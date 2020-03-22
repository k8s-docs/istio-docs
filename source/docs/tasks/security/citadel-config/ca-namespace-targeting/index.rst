ca-namespace-targeting
=========================

A cluster operator might decide not to generate ``ServiceAccount``
secrets for some subset of namespaces, or to make ``ServiceAccount``
secret generation opt-in per namespace. This task describes how an
operator can configure their cluster for these situations. Full
documentation of the Citadel namespace targeting mechanism can be found
`here </docs/ops/configuration/mesh/secret-creation/>`_.

Before you begin
----------------

To complete this task, you should first take the following actions:

-  Read the `security
   concept </docs/ops/configuration/mesh/secret-creation/>`_.

-  Follow the `Istio installation
   guide </docs/setup/install/istioctl/>`_ to install Istio with mutual
   TLS enabled.

Deactivating service account secret generation for a single namespace
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To create a new sample namespace ``foo``, run:

.. code:: sh

      $ kubectl create ns foo

Service account secrets are created following the default behavior. To
verify that Citadel has generated a key/cert secret for the default
service account in the ``foo`` namespace, run (note that this may take
up to 1 minute):

.. code:: sh

      $ kubectl get secrets -n foo \| grep istio.io NAME
TYPE DATA AGE istio.default istio.io/key-and-cert 3 13s

To label the namespace to prevent Citadel from creating
``ServiceAccount`` secrets in target namespace ``foo``, run:

.. code:: sh

      $ kubectl label ns foo ca.istio.io/override=false

To create a new ``ServiceAccount`` in this namespace, run:

.. code:: sh

      $ kubectl apply -f - <<EOF apiVersion: v1 kind:
ServiceAccount metadata: name: sample-service-account namespace: foo EOF


To check the namespace’s secrets again, run:

.. code:: sh

      $ kubectl get secrets -n foo \| grep istio.io NAME
TYPE DATA AGE istio.default istio.io/key-and-cert 3 11m

You can observe that no new ``istio.io/key-and-cert`` secret was
generated for the ``sample-service-account`` service account.

Opt-in service account secret generation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Set the ``enableNamespacesByDefault`` installation option to ``false``
to make ``ServiceAcount`` secret generation opt-in (i.e., to disable
generating secrets unless otherwise specified):

.. code:: yaml

    … security: enableNamespacesByDefault: false …

Once this mesh configuration is applied, to create a namespace ``foo``
and check the secrets present in that namespace, run:

.. code:: sh

      $ kubectl create ns foo $ kubectl get secrets -n foo
\| grep istio.io

You can observe that no secrets have been created. To override this
value for the ``foo`` namespace, add a ``ca.istio.io/override=true``
label in that namespace:

.. code:: sh

      $ kubectl label ns foo ca.istio.io/override=true

To create a new service account in the ``foo`` namespace, run:

.. code:: sh

      $ kubectl apply -f - <<EOF apiVersion: v1 kind:
ServiceAccount metadata: name: sample-service-account namespace: foo EOF


To check the secrets in the ``foo`` namespace again, run:

.. code:: sh

      $ kubectl get secrets -n foo \| grep istio.io NAME
TYPE DATA AGE istio.default istio.io/key-and-cert 3 47s
istio.sample-service-account istio.io/key-and-cert 3 6s

You can observe that an ``istio.io/key-and-cert`` secret has been
created for the ``default`` service account in addition to the
``sample-service-account``. This is due to the retroactive secret
generation feature, which will create secrets for all service accounts
in a namespace once it transitions from inactive to active.

Cleanup
-------

To delete the ``foo`` test namespace and all its resources, run:

.. code:: sh

      $ kubectl delete ns foo
