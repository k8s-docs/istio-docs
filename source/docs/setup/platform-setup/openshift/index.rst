OpenShift
============================

.. warning::

   OpenShift 4.1 and above use ``nftables``, which is
incompatible with the Istio ``proxy-init`` container. Make sure to use
`CNI </docs/setup/additional-setup/cni/>`_ instead.

Follow these instructions to prepare an OpenShift cluster for Istio.

By default, OpenShift doesn’t allow containers running with user ID 0.
You must enable containers running with UID 0 for Istio’s service
accounts by running the command below. Make sure to replace
``istio-system`` if you are deploying Istio in another namespace:

.. code:: sh

      $ oc adm policy add-scc-to-group anyuid
system:serviceaccounts:istio-system

Now you can install Istio using the
`CNI </docs/setup/additional-setup/cni/>`_ instructions.

After installation is complete, expose an OpenShift route for the
ingress gateway.

.. code:: sh

      $ oc -n istio-system expose svc/istio-ingressgateway
–port=http2

Automatic sidecar injection
---------------------------

.. note::

   This setup is not necessary if you are running OpenShift 4.1
or higher. If this is the case, skip to the next section.

Webhook and certificate signing requests support must be enabled for
`automatic
injection </docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection>`_
to work. Modify the master configuration file on the master node for the
cluster as follows.

.. note::

   By default, the master configuration file can be found in
``/etc/origin/master/master-config.yaml``.

In the same directory as the master configuration file, create a file
named ``master-config.patch`` with the following contents:

.. code:: yaml

    admissionConfig: pluginConfig:
MutatingAdmissionWebhook: configuration: apiVersion:
apiserver.config.k8s.io/v1alpha1 kubeConfigFile: /dev/null kind:
WebhookAdmission ValidatingAdmissionWebhook: configuration: apiVersion:
apiserver.config.k8s.io/v1alpha1 kubeConfigFile: /dev/null kind:
WebhookAdmission

In the same directory, execute:

.. code:: sh

      $ cp -p master-config.yaml master-config.yaml.prepatch
$ oc ex config patch master-config.yaml.prepatch -p “$(cat
master-config.patch)” > master-config.yaml $ master-restart api $
master-restart controllers

Privileged security context constraints for application sidecars
----------------------------------------------------------------

The Istio sidecar injected into each application pod runs with user ID
1337, which is not allowed by default in OpenShift. To allow this user
ID to be used, execute the following commands. Replace
``<target-namespace>`` with the appropriate namespace.

.. code:: sh

      $ oc adm policy add-scc-to-group privileged
system:serviceaccounts: $ oc adm policy add-scc-to-group anyuid
system:serviceaccounts:

When removing your application, remove the permissions as follows.

.. code:: sh

      $ oc adm policy remove-scc-from-group privileged
system:serviceaccounts: $ oc adm policy remove-scc-from-group anyuid
system:serviceaccounts:

Additional requirements for the application namespace
-----------------------------------------------------

CNI on OpenShift is managed by ``Multus``, and it requires a
``NetworkAttachmentDefinition`` to be present in the application
namespace in order to invoke the ``istio-cni`` plugin. Execute the
following commands. Replace ``<target-namespace>`` with the appropriate
namespace.

.. code:: sh

      $ cat <<EOF \| oc -n create -f - apiVersion:
“k8s.cni.cncf.io/v1” kind: NetworkAttachmentDefinition metadata: name:
istio-cni EOF

When removing your application, remove the
``NetworkAttachmentDefinition`` as follows.

.. code:: sh

      $ oc -n delete network-attachment-definition istio-cni

