mtls-migration
==========================

This task shows how to ensure your workloads only communicate using
mutual TLS as they are migrated to Istio.

Istio automatically configures workload sidecars to use `mutual
TLS </docs/tasks/security/authentication/authn-policy/#auto-mutual-tls>`_
when calling other workloads. By default, Istio configures the
destination workloads using ``PERMISSIVE`` mode. When ``PERMISSIVE``
mode is enabled, a service can accept both plain text and mutual TLS
traffic. In order to only allow mutual TLS traffic, the configuration
needs to be changed to ``STRICT`` mode.

You can use the `Grafana
dashboard </docs/tasks/observability/metrics/using-istio-dashboard/>`_
to check which workloads are still sending plaintext traffic to the
workloads in ``PERMISSIVE`` mode and choose to lock them down once the
migration is done.

Before you begin
----------------

.. raw:: html

   <!-- TODO: update the link after other PRs are merged -->

-  Understand Istio `authentication
   policy </docs/concepts/security/#authentication-policies>`_ and
   related `mutual TLS
   authentication </docs/concepts/security/#mutual-tls-authentication>`_
   concepts.

-  Read the `authentication policy
   task </docs/tasks/security/authentication/authn-policy>`_ to learn
   how to configure authentication policy.

-  Have a Kubernetes cluster with Istio installed, without global mutual
   TLS enabled (e.g use the demo configuration profile as described in
   `installation steps </docs/setup/getting-started>`_).

In this task, you can try out the migration process by creating sample
workloads and modifying the policies to enforce STRICT mutual TLS
between the workloads.

Set up the cluster
------------------

-  Create two namespaces, ``foo`` and ``bar``, and deploy
   `httpbin <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/httpbin>`_ and
   `sleep <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/sleep>`_ with
   sidecars on both of them:

   .. code:: sh

      $ kubectl create ns foo $ kubectl apply -f
   <(istioctl kube-inject -f @samples/httpbin/httpbin.yaml@) -n foo $
   kubectl apply -f <(istioctl kube-inject -f
   @samples/sleep/sleep.yaml@) -n foo $ kubectl create ns bar $ kubectl
   apply -f <(istioctl kube-inject -f @samples/httpbin/httpbin.yaml@) -n
   bar $ kubectl apply -f <(istioctl kube-inject -f
   @samples/sleep/sleep.yaml@) -n bar

-  Create another namespace, ``legacy``, and deploy
   `sleep <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/sleep>`_ without
   a sidecar:

   .. code:: sh

      $ kubectl create ns legacy $ kubectl apply -f
   @samples/sleep/sleep.yaml@ -n legacy

-  Verify setup by sending an http request (using curl command) from any
   sleep pod (among those in namespace ``foo``, ``bar`` or ``legacy``)
   to ``httpbin.foo``. All requests should success with HTTP code 200.

   .. code:: sh

      $ for from in “foo” “bar” “legacy”; do for to in
   “foo” “bar”; do kubectl exec $(kubectl get pod -l app=sleep -n
   ${from} -o jsonpath={.items..metadata.name}) -c sleep -n
   :math:`{from} -- curl http://httpbin.`\ {to}:8000/ip -s -o /dev/null
   -w “sleep.\ :math:`{from} to httpbin.`\ {to}:
   %{http_code}:raw-latex:`\n`”; done; done sleep.foo to httpbin.foo:
   200 sleep.foo to httpbin.bar: 200 sleep.bar to httpbin.foo: 200
   sleep.bar to httpbin.bar: 200 sleep.legacy to httpbin.foo: 200
   sleep.legacy to httpbin.bar: 200

-  Also verify that there are no authentication policies or destination
   rules (except control plane ones) in the system:

   .. code:: sh

      $ kubectl get peerauthentication –all-namespaces No
   resources found

   .. code:: sh

      $ kubectl get destinationrule –all-namespaces No
   resources found

Lock down to mutual TLS by namespace
------------------------------------

After migrating all clients to Istio and injecting the Envoy sidecar,
you can lock down workloads in the ``foo`` namespace to only accept
mutual TLS traffic.

.. code:: sh

      $ kubectl apply -n foo -f - <<EOF apiVersion:
“security.istio.io/v1beta1” kind: “PeerAuthentication” metadata: name:
“default” spec: mtls: mode: STRICT EOF

Now, you should see the request from ``sleep.legacy`` to ``httpbin.foo``
failing.

.. code:: sh

      $ for from in “foo” “bar” “legacy”; do for to in “foo”
“bar”; do kubectl exec $(kubectl get pod -l app=sleep -n ${from} -o
jsonpath={.items..metadata.name}) -c sleep -n
:math:`{from} -- curl http://httpbin.`\ {to}:8000/ip -s -o /dev/null -w
“sleep.\ :math:`{from} to httpbin.`\ {to}: %{http_code}:raw-latex:`\n`”;
done; done sleep.foo to httpbin.foo: 200 sleep.foo to httpbin.bar: 200
sleep.bar to httpbin.foo: 200 sleep.bar to httpbin.bar: 200 sleep.legacy
to httpbin.foo: 000 command terminated with exit code 56 sleep.legacy to
httpbin.bar: 200

If you can’t migrate all your services to Istio (i.e., inject Envoy
sidecar in all of them), you will need to continue to use ``PERMISSIVE``
mode. However, when configured with ``PERMISSIVE`` mode, no
authentication or authorization checks will be performed for plaintext
traffic by default. We recommend you use `Istio
Authorization </docs/tasks/security/authorization/authz-http/>`_ to
configure different paths with different authorization policies.

Lock down mutual TLS for the entire mesh
----------------------------------------

.. code:: sh

      $ kubectl apply -n istio-system -f - <<EOF apiVersion:
“security.istio.io/v1beta1” kind: “PeerAuthentication” metadata: name:
“default” spec: mtls: mode: STRICT EOF

Now, both the ``foo`` and ``bar`` namespaces enforce mutual TLS only
traffic, so you should see requests from ``sleep.legacy`` failing for
both.

.. code:: sh

      $ for from in “foo” “bar” “legacy”; do for to in “foo”
“bar”; do kubectl exec $(kubectl get pod -l app=sleep -n ${from} -o
jsonpath={.items..metadata.name}) -c sleep -n
:math:`{from} -- curl http://httpbin.`\ {to}:8000/ip -s -o /dev/null -w
“sleep.\ :math:`{from} to httpbin.`\ {to}: %{http_code}:raw-latex:`\n`”;
done; done

Clean up the example
--------------------

1. To remove all authentication policies

.. code:: sh

      $ kubectl delete peerauthentication –all-namespaces
–all

1. If you are not planning to explore any follow-on tasks, you can
   remove all test namespaces.

.. code:: sh

      $ kubectl delete ns foo bar legacy Namespaces foo bar
legacy deleted.
