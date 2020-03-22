Upgrade using Helm
============================

Follow this guide to upgrade the Istio control plane and sidecar proxies
of an existing Istio deployment that was previously installed using
Helm. The upgrade process may install new binaries and may change
configuration and API schemas. The upgrade process may result in service
downtime. To minimize downtime, please ensure your Istio control plane
components and your applications are highly available with multiple
replicas.

.. warning::

   Be sure to check out the `upgrade
notes </news/releases/%7B%7B%3C%20istio_version%20%3E%7D%7D.x/announcing-%7B%7B%3C%20istio_version%20%3E%7D%7D/upgrade-notes>`_
for a concise list of things you should know before upgrading your
deployment to Istio {{< istio_version >}}.

.. note::

   Istio does **NOT** support skip level upgrades. Only
upgrades from {{< istio_previous_version >}} to {{< istio_version >}}
are supported. If you are on an older version, please upgrade to {{<
istio_previous_version >}} first.

Upgrade steps
-------------

`Download the new Istio
release </docs/setup/getting-started/#download>`_ and change directory
to the new release directory.

Istio CNI upgrade
~~~~~~~~~~~~~~~~~

If you have installed or are planning to install `Istio
CNI </docs/setup/additional-setup/cni/>`_, choose one of the following
**mutually exclusive** options to check whether Istio CNI is already
installed and to upgrade it:

{{< tabset category-name=“controlplaneupdate” >}} {{< tab
name=“Kubernetes rolling update” category-value=“k8supdate” >}}

You can use Kubernetes’ rolling update mechanism to upgrade the Istio
CNI components. This is suitable for cases where ``kubectl apply`` was
used to deploy Istio CNI.

1. To check whether ``istio-cni`` is installed, search for
   ``istio-cni-node`` pods and in which namespace they are running
   (typically, ``kube-system`` or ``istio-system``):

   .. code:: sh

      $ kubectl get pods -l k8s-app=istio-cni-node
   –all-namespaces $ NAMESPACE=$(kubectl get pods -l
   k8s-app=istio-cni-node –all-namespaces
   –output=‘jsonpath={.items[0].metadata.namespace}’)

2. If ``istio-cni`` is currently installed in a namespace other than
   ``kube-system`` (for example, ``istio-system``), delete
   ``istio-cni``:

   .. code:: sh

      $ helm template install/kubernetes/helm/istio-cni
   –name=istio-cni –namespace=$NAMESPACE \| kubectl delete -f -

3. Install or upgrade ``istio-cni`` in the ``kube-system`` namespace:

   .. code:: sh

      $ helm template install/kubernetes/helm/istio-cni
   –name=istio-cni –namespace=kube-system \| kubectl apply -f -

{{< /tab >}}

{{< tab name=“Helm upgrade” category-value=“helmupgrade” >}}

If you installed Istio CNI using `Helm and
Tiller </docs/setup/install/helm/#option-2-install-with-helm-and-tiller-via-helm-install>`_,
the preferred upgrade option is to let Helm take care of the upgrade.

1. Check whether ``istio-cni`` is installed, and in which namespace:

   .. code:: sh

      $ helm status istio-cni

2. (Re-)install or upgrade ``istio-cni`` depending on the status:

   -  If ``istio-cni`` is not currently installed and you decide to
      install it:

      .. code:: sh

      $ helm install install/kubernetes/helm/istio-cni
      –name istio-cni –namespace kube-system

   -  If ``istio-cni`` is currently installed in a namespace other than
      ``kube-system`` (for example, ``istio-system``), delete it:

      .. code:: sh

      $ helm delete –purge istio-cni

      Then install it again in the ``kube-system`` namespace:

      .. code:: sh

      $ helm install install/kubernetes/helm/istio-cni
      –name istio-cni –namespace kube-system

   -  If ``istio-cni`` is currently installed in the ``kube-system``
      namespace, upgrade it:

      .. code:: sh

      $ helm upgrade istio-cni
      install/kubernetes/helm/istio-cni –namespace kube-system {{< /text
      >}}

{{< /tab >}} {{< /tabset >}}

Control plane upgrade
~~~~~~~~~~~~~~~~~~~~~

Pilot, Galley, Policy, Telemetry and Sidecar injector. Choose one of the
following **mutually exclusive** options to update the control plane:

{{< tabset category-name=“controlplaneupdate” >}} {{< tab
name=“Kubernetes rolling update” category-value=“k8supdate” >}} You can
use Kubernetes’ rolling update mechanism to upgrade the control plane
components. This is suitable for cases where ``kubectl apply`` was used
to deploy the Istio components, including configurations generated using
`helm
template </docs/setup/install/helm/#option-1-install-with-helm-via-helm-template>`_.

1. Use ``kubectl apply`` to upgrade all of Istio’s CRDs. Wait a few
   seconds for the Kubernetes API server to commit the upgraded CRDs:

   .. code:: sh

      $ kubectl apply -f
   install/kubernetes/helm/istio-init/files/

2. {{< boilerplate verify-crds >}}

3. Apply the update templates:

   | .. code:: sh

      $ helm template install/kubernetes/helm/istio
     –name istio
   | –namespace istio-system \| kubectl apply -f -

   You must pass the same settings as when you first `installed
   Istio </docs/setup/install/helm>`_.

The rolling update process will upgrade all deployments and configmaps
to the new version. After this process finishes, your Istio control
plane should be updated to the new version. Your existing application
should continue to work without any change. If there is any critical
issue with the new control plane, you can rollback the changes by
applying the yaml files from the old version. {{< /tab >}}

{{< tab name=“Helm upgrade” category-value=“helmupgrade” >}} If you
installed Istio using `Helm and
Tiller </docs/setup/install/helm/#option-2-install-with-helm-and-tiller-via-helm-install>`_,
the preferred upgrade option is to let Helm take care of the upgrade.

1. Upgrade the ``istio-init`` chart to update all the Istio `Custom
   Resource
   Definitions <https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions>`_
   (CRDs).

   .. code:: sh

      $ helm upgrade –install istio-init
   install/kubernetes/helm/istio-init –namespace istio-system {{< /text
   >}}

2. {{< boilerplate verify-crds >}}

3. Upgrade the ``istio`` chart:

   .. code:: sh

      $ helm upgrade istio install/kubernetes/helm/istio
   –namespace istio-system

   If Istio CNI is installed, enable it by adding the
   ``--set istio_cni.enabled=true`` setting.

{{< /tab >}} {{< /tabset >}}

Sidecar upgrade
~~~~~~~~~~~~~~~

After the control plane upgrade, the applications already running Istio
will still be using an older sidecar. To upgrade the sidecar, you will
need to re-inject it.

If you’re using automatic sidecar injection, you can upgrade the sidecar
by doing a rolling update for all the pods, so that the new version of
the sidecar will be automatically re-injected.

.. warning::

   Your ``kubectl`` version must be >= 1.15 to run the
following command. Upgrade if necessary.

.. code:: sh

      $ kubectl rollout restart deployment –namespace
default

If you’re using manual injection, you can upgrade the sidecar by
executing:

.. code:: sh

      $ kubectl apply -f <(istioctl kube-inject -f
$ORIGINAL_DEPLOYMENT_YAML)

If the sidecar was previously injected with some customized inject
configuration files, you will need to change the version tag in the
configuration files to the new version and re-inject the sidecar as
follows:

| .. code:: sh

      $ kubectl apply -f <(istioctl kube-inject
| –injectConfigFile inject-config.yaml
| –filename $ORIGINAL_DEPLOYMENT_YAML)
