v1beta1-authorization-policy
================================================

Istio 1.4 introduces the `v1beta1 authorization
policy </docs/reference/config/security/authorization-policy/>`_, which
is a major update to the previous ``v1alpha1`` role-based access control
(RBAC) policy. The new policy provides these improvements:

-  Aligns with Istio configuration model.
-  Improves the user experience by simplifying the API.
-  Supports more use cases (e.g. Ingress/Egress gateway support) without
   added complexity.

The ``v1beta1`` policy is not backward compatible and requires a one
time conversion. A tool is provided to automate this process. The
previous configuration resources ``ClusterRbacConfig``, ``ServiceRole``,
and ``ServiceRoleBinding`` will not be supported from Istio 1.6 onwards.

This post describes the new ``v1beta1`` authorization policy model, its
design goals and the migration from ``v1alpha1`` RBAC policies. See the
`authorization concept page </docs/concepts/security/#authorization>`_
for a detailed in-depth explanation of the ``v1beta1`` authorization
policy.

We welcome your feedback about the ``v1beta1`` authorization policy at
`discuss.istio.io <https://discuss.istio.io/c/security>`_.

Background
----------

To date, Istio provided RBAC policies to enforce access control on {{<
gloss “service” >}}services{{< /gloss >}} using three configuration
resources: ``ClusterRbacConfig``, ``ServiceRole`` and
``ServiceRoleBinding``. With this API, users have been able to enforce
control access at mesh-level, namespace-level and service-level. Like
other RBAC policies, Istio RBAC uses the same concept of role and
binding for granting permissions to identities.

Although Istio RBAC has been working reliably, we’ve found that many
improvements were possible.

For example, users have mistakenly assumed that access control
enforcement happens at service-level because ``ServiceRole`` uses
service to specify where to apply the policy, however, the policy is
actually applied on {{< gloss “workload” >}}workloads{{< /gloss >}}, the
service is only used to find the corresponding workload. This nuance is
significant when multiple services are referring to the same workload. A
``ServiceRole`` for service A will also affect service B if the two
services are referring to the same workload, which can cause confusion
and incorrect configuration.

An other example is that it’s proven difficult for users to maintain and
manage the Istio RBAC configurations because of the need to deeply
understand three related resources.

Design goals
------------

The new ``v1beta1`` authorization policy had several design goals:

-  Align with `Istio Configuration Model <https://goo.gl/x3STjD>`_ for
   better clarity on the policy target. The configuration model provides
   a unified configuration hierarchy, resolution and target selection.

-  Improve the user experience by simplifying the API. It’s easier to
   manage one custom resource definition (CRD) that includes all access
   control specifications, instead of multiple CRDs.

-  Support more use cases without added complexity. For example, allow
   the policy to be applied on Ingress/Egress gateway to enforce access
   control for traffic entering/exiting the mesh.

``AuthorizationPolicy``
-----------------------

An `AuthorizationPolicy custom
resource </docs/reference/config/security/authorization-policy/>`_
enables access control on workloads. This section gives an overview of
the changes in the ``v1beta1`` authorization policy.

An ``AuthorizationPolicy`` includes a ``selector`` and a list of
``rule``. The ``selector`` specifies on which workload to apply the
policy and the list of ``rule`` specifies the detailed access control
rule for the workload.

The ``rule`` is additive, which means a request is allowed if any
``rule`` allows the request. Each ``rule`` includes a list of ``from``,
``to`` and ``when``, which specifies **who** is allowed to do **what**
under which **conditions**.

The ``selector`` replaces the functionality provided by
``ClusterRbacConfig`` and the ``services`` field in ``ServiceRole``. The
``rule`` replaces the other fields in the ``ServiceRole`` and
``ServiceRoleBinding``.

Example
~~~~~~~

The following authorization policy applies to workloads with
``app: httpbin`` and ``version: v1`` label in the ``foo`` namespace:

.. code:: yaml

    apiVersion: security.istio.io/v1beta1 kind:
AuthorizationPolicy metadata: name: httpbin namespace: foo spec:
selector: matchLabels: app: httpbin version: v1 rules: - from: - source:
principals: [“cluster.local/ns/default/sa/sleep”] to: - operation:
methods: [“GET”] when: - key: request.headers[version] values: [“v1”,
“v2”]

The policy allows principal ``cluster.local/ns/default/sa/sleep`` to
access the workload using the ``GET`` method when the request includes a
``version`` header of value ``v1`` or ``v2``. Any requests not matched
with the policy will be denied by default.

Assuming the ``httpbin`` service is defined as:

.. code:: yaml

    apiVersion: v1 kind: Service metadata: name: httpbin
namespace: foo spec: selector: app: httpbin version: v1 ports: # omitted


You would need to configure three resources to achieve the same result
in ``v1alpha1``:

.. code:: yaml

    apiVersion: “rbac.istio.io/v1alpha1” kind:
ClusterRbacConfig metadata: name: default spec: mode:
‘ON_WITH_INCLUSION’ inclusion: services:
[“httpbin.foo.svc.cluster.local”] — apiVersion: “rbac.istio.io/v1alpha1”
kind: ServiceRole metadata: name: httpbin namespace: foo spec: rules: -
services: [“httpbin.foo.svc.cluster.local”] methods: [“GET”]
constraints: - key: request.headers[version] values: [“v1”, “v2”] —
apiVersion: “rbac.istio.io/v1alpha1” kind: ServiceRoleBinding metadata:
name: httpbin namespace: foo spec: subjects: - user:
“cluster.local/ns/default/sa/sleep” roleRef: kind: ServiceRole name:
“httpbin”

Workload selector
~~~~~~~~~~~~~~~~~

A major change in the ``v1beta1`` authorization policy is that it now
uses workload selector to specify where to apply the policy. This is the
same workload selector used in the ``Gateway``, ``Sidecar`` and
``EnvoyFilter`` configurations.

The workload selector makes it clear that the policy is applied and
enforced on workloads instead of services. If a policy applies to a
workload that is used by multiple different services, the same policy
will affect the traffic to all the different services.

You can simply leave the ``selector`` empty to apply the policy to all
workloads in a namespace. The following policy applies to all workloads
in the namespace ``bar``:

.. code:: yaml

    apiVersion: security.istio.io/v1beta1 kind:
AuthorizationPolicy metadata: name: policy namespace: bar spec: rules: #
omitted

Root namespace
~~~~~~~~~~~~~~

A policy in the root namespace applies to all workloads in the mesh in
every namespaces. The root namespace is configurable in the
```MeshConfig`` </docs/reference/config/istio.mesh.v1alpha1/#MeshConfig>`_
and has the default value of ``istio-system``.

For example, you installed Istio in ``istio-system`` namespace and
deployed workloads in ``default`` and ``bookinfo`` namespace. The root
namespace is changed to ``istio-config`` from the default value. The
following policy will apply to workloads in every namespace including
``default``, ``bookinfo`` and the ``istio-system``:

.. code:: yaml

    apiVersion: security.istio.io/v1beta1 kind:
AuthorizationPolicy metadata: name: policy namespace: istio-config spec:
rules: # omitted

Ingress/Egress Gateway support
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``v1beta1`` authorization policy can also be applied on
ingress/egress gateway to enforce access control on traffic
entering/leaving the mesh, you only need to change the ``selector`` to
make select the ingress/egress workload.

The following policy applies to workloads with the
``app: istio-ingressgateway`` label:

.. code:: yaml

    apiVersion: security.istio.io/v1beta1 kind:
AuthorizationPolicy metadata: name: ingress namespace: istio-system
spec: selector: matchLabels: app: istio-ingressgateway rules: # omitted


Remember the authorization policy only applies to workloads in the same
namespace as the policy, unless the policy is applied in the root
namespace:

-  If you don’t change the default root namespace value
   (i.e. ``istio-system``), the above policy will apply to workloads
   with the ``app: istio-ingressgateway`` label in **every** namespace.

-  If you have changed the root namespace to a different value, the
   above policy will only apply to workloads with the
   ``app: istio-ingressgateway`` label **only** in the ``istio-system``
   namespace.

Comparison
~~~~~~~~~~

The following table highlights the key differences between the old
``v1alpha1`` RBAC policies and the new ``v1beta1`` authorization policy.

Feature
^^^^^^^

+--------+-------------------------+----------------------------------+
| Featur | ``v1alpha1`` RBAC       | ``v1beta1`` Authorization Policy |
| e      | policy                  |                                  |
+========+=========================+==================================+
| API    | ``alpha``: **No**       | ``beta``: backward compatible    |
| stabil | backward compatible     | **guaranteed**                   |
| ity    |                         |                                  |
+--------+-------------------------+----------------------------------+
| Number | Three:                  | Only One:                        |
| of     | ``ClusterRbacConfig``,  | ``AuthorizationPolicy``          |
| CRDs   | ``ServiceRole`` and     |                                  |
|        | ``ServiceRoleBinding``  |                                  |
+--------+-------------------------+----------------------------------+
| Policy | **service**             | **workload**                     |
| target |                         |                                  |
+--------+-------------------------+----------------------------------+
| Deny-b | Enabled **explicitly**  | Enabled **implicitly** with      |
| y-defa | by configuring          | ``AuthorizationPolicy``          |
| ult    | ``ClusterRbacConfig``   |                                  |
| behavi |                         |                                  |
| or     |                         |                                  |
+--------+-------------------------+----------------------------------+
| Ingres | Not supported           | Supported                        |
| s/Egre |                         |                                  |
| ss     |                         |                                  |
| gatewa |                         |                                  |
| y      |                         |                                  |
| suppor |                         |                                  |
| t      |                         |                                  |
+--------+-------------------------+----------------------------------+
| The    | Match all contents      | Match non-empty contents only    |
| ``"*"` | (empty and non-empty)   |                                  |
| `      |                         |                                  |
| value  |                         |                                  |
| in     |                         |                                  |
| policy |                         |                                  |
+--------+-------------------------+----------------------------------+

The following tables show the relationship between the ``v1alpha1`` and
``v1beta1`` API.

``ClusterRbacConfig``
^^^^^^^^^^^^^^^^^^^^^

+---------------------------------+------------------------------------+
| ``ClusterRbacConfig.Mode``      | ``AuthorizationPolicy``            |
+=================================+====================================+
| ``OFF``                         | No policy applied                  |
+---------------------------------+------------------------------------+
| ``ON``                          | A deny-all policy applied in root  |
|                                 | namespace                          |
+---------------------------------+------------------------------------+
| ``ON_WITH_INCLUSION``           | policies should be applied to      |
|                                 | namespaces or workloads included   |
|                                 | by ``ClusterRbacConfig``           |
+---------------------------------+------------------------------------+
| ``ON_WITH_EXCLUSION``           | policies should be applied to      |
|                                 | namespaces or workloads excluded   |
|                                 | by ``ClusterRbacConfig``           |
+---------------------------------+------------------------------------+

``ServiceRole``
^^^^^^^^^^^^^^^

+---------------------------+------------------------------------------+
| ``ServiceRole``           | ``AuthorizationPolicy``                  |
+===========================+==========================================+
| ``services``              | ``selector``                             |
+---------------------------+------------------------------------------+
| ``paths``                 | ``paths`` in ``to``                      |
+---------------------------+------------------------------------------+
| ``methods``               | ``methods`` in ``to``                    |
+---------------------------+------------------------------------------+
| ``destination.ip`` in     | Not supported                            |
| constraint                |                                          |
+---------------------------+------------------------------------------+
| ``destination.port`` in   | ``ports`` in ``to``                      |
| constraint                |                                          |
+---------------------------+------------------------------------------+
| ``destination.labels`` in | ``selector``                             |
| constraint                |                                          |
+---------------------------+------------------------------------------+
| ``destination.namespace`` | Replaced by the namespace of the policy, |
| in constraint             | i.e. the ``namespace`` in metadata       |
+---------------------------+------------------------------------------+
| ``destination.user`` in   | Not supported                            |
| constraint                |                                          |
+---------------------------+------------------------------------------+
| ``experimental.envoy.filt | ``experimental.envoy.filters`` in        |
| ers``                     | ``when``                                 |
| in constraint             |                                          |
+---------------------------+------------------------------------------+
| ``request.headers`` in    | ``request.headers`` in ``when``          |
| constraint                |                                          |
+---------------------------+------------------------------------------+

``ServiceRoleBinding``
^^^^^^^^^^^^^^^^^^^^^^

+----------------------------------+-----------------------------------+
| ``ServiceRoleBinding``           | ``AuthorizationPolicy``           |
+==================================+===================================+
| ``user``                         | ``principals`` in ``from``        |
+----------------------------------+-----------------------------------+
| ``group``                        | ``request.auth.claims[group]`` in |
|                                  | ``when``                          |
+----------------------------------+-----------------------------------+
| ``source.ip`` in property        | ``ipBlocks`` in ``from``          |
+----------------------------------+-----------------------------------+
| ``source.namespace`` in property | ``namespaces`` in ``from``        |
+----------------------------------+-----------------------------------+
| ``source.principal`` in property | ``principals`` in ``from``        |
+----------------------------------+-----------------------------------+
| ``request.headers`` in property  | ``request.headers`` in ``when``   |
+----------------------------------+-----------------------------------+
| ``request.auth.principal`` in    | ``requestPrincipals`` in ``from`` |
| property                         | or ``request.auth.principal`` in  |
|                                  | ``when``                          |
+----------------------------------+-----------------------------------+
| ``request.auth.audiences`` in    | ``request.auth.audiences`` in     |
| property                         | ``when``                          |
+----------------------------------+-----------------------------------+
| ``request.auth.presenter`` in    | ``request.auth.presenter`` in     |
| property                         | ``when``                          |
+----------------------------------+-----------------------------------+
| ``request.auth.claims`` in       | ``request.auth.claims`` in        |
| property                         | ``when``                          |
+----------------------------------+-----------------------------------+

Beyond all the differences, the ``v1beta1`` policy is enforced by the
same engine in Envoy and supports the same authenticated identity
(mutual TLS or JWT), condition and other primitives (e.g. IP, port and
etc.) as the ``v1alpha1`` policy.

Future of the ``v1alpha1`` policy
---------------------------------

The ``v1alpha1`` RBAC policy (``ClusterRbacConfig``, ``ServiceRole``,
and ``ServiceRoleBinding``) is deprecated by the ``v1beta1``
authorization policy.

Istio 1.4 continues to support the ``v1alpha1`` RBAC policy to give you
enough time to move away from the alpha policies.

Migration from the ``v1alpha1`` policy
--------------------------------------

Istio only supports one of the two versions for a given workload:

-  If there is only ``v1beta1`` policy for a workload, the ``v1beta1``
   policy will be used.
-  If there is only ``v1alpha1`` policy for a workload, the ``v1alpha1``
   policy will be used.
-  If there are both ``v1beta1`` and ``v1alpha1`` policies for a
   workload, only the ``v1beta1`` policy will be used and the the
   ``v1alpha1`` policy will be ignored.

General Guideline
~~~~~~~~~~~~~~~~~

.. warning::

   When migrating to use ``v1beta1`` policy for a given
workload, make sure the new ``v1beta1`` policy covers all the existing
``v1alpha1`` policies applied for the workload, because the ``v1alpha1``
policies applied for the workload will be ignored after you applied the
``v1beta1`` policies.

The typical flow of migrating to ``v1beta1`` policy is to start by
checking the ``ClusterRbacConfig`` to decide which namespace or service
is enabled with RBAC.

For each service enabled with RBAC:

1. Get the workload selector from the service definition.
2. Create a ``v1beta1`` policy with the workload selector.
3. Update the ``v1beta1`` policy for each ``ServiceRole`` and
   ``ServiceRoleBinding`` applied to the service.
4. Apply the ``v1beta1`` policy and monitor the traffic to make sure the
   policy is working as expected.
5. Repeat the process for the next service enabled with RBAC.

For each namespace enabled with RBAC:

1. Apply a ``v1beta1`` policy that denies all traffic to the given
   namespace.

Migration Example
~~~~~~~~~~~~~~~~~

Assume you have the following ``v1alpha1`` policies for the ``httpbin``
service in the ``foo`` namespace:

.. code:: yaml

    apiVersion: “rbac.istio.io/v1alpha1” kind:
ClusterRbacConfig metadata: name: default spec: mode:
‘ON_WITH_INCLUSION’ inclusion: namespaces: [“foo”] — apiVersion:
“rbac.istio.io/v1alpha1” kind: ServiceRole metadata: name: httpbin
namespace: foo spec: rules: - services:
[“httpbin.foo.svc.cluster.local”] methods: [“GET”] — apiVersion:
“rbac.istio.io/v1alpha1” kind: ServiceRoleBinding metadata: name:
httpbin namespace: foo spec: subjects: - user:
“cluster.local/ns/default/sa/sleep” roleRef: kind: ServiceRole name:
“httpbin”

Migrate the above policies to ``v1beta1`` in the following ways:

1. Assume the ``httpbin`` service has the following workload selector:

   .. code:: yaml

    selector: app: httpbin version: v1

2. Create a ``v1beta1`` policy with the workload selector:

   .. code:: yaml

    apiVersion: security.istio.io/v1beta1 kind:
   AuthorizationPolicy metadata: name: httpbin namespace: foo spec:
   selector: matchLabels: app: httpbin version: v1

3. Update the ``v1beta1`` policy with each ``ServiceRole`` and
   ``ServiceRoleBinding`` applied to the service:

   .. code:: yaml

    apiVersion: security.istio.io/v1beta1 kind:
   AuthorizationPolicy metadata: name: httpbin namespace: foo spec:
   selector: matchLabels: app: httpbin version: v1 rules:

   -  from:

      -  source: principals: [“cluster.local/ns/default/sa/sleep”] to:
      -  operation: methods: [“GET”]

4. Apply the ``v1beta1`` policy and monitor the traffic to make sure it
   works as expected.

5. Apply the following ``v1beta1`` policy that denies all traffic to the
   ``foo`` namespace because the ``foo`` namespace is enabled with RBAC:

   .. code:: yaml

    apiVersion: security.istio.io/v1beta1 kind:
   AuthorizationPolicy metadata: name: deny-all namespace: foo spec: {}


Make sure the ``v1beta1`` policy is working as expected and then you can
delete the ``v1alpha1`` policies from the cluster.

Automation of the Migration
~~~~~~~~~~~~~~~~~~~~~~~~~~~

To help ease the migration, the ``istioctl experimental authz convert``
command is provided to automatically convert the ``v1alpha1`` policies
to the ``v1beta1`` policy.

You can evaluate the command but it is experimental in Istio 1.4 and
doesn’t support the full ``v1alpha1`` semantics as of the date of this
blog post.

The command to support the full ``v1alpha1`` semantics is expected in a
patch release following Istio 1.4.
