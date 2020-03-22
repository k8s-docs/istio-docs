Install Istion with the Istio CNI plugin
==============================================

Follow this guide to install, configure, and use an Istio mesh using the
Istio Container Network Interface
(`CNI <https://github.com/containernetworking/cni#cni---the-container-network-interface>`_)
plugin.

By default Istio injects an ``initContainer``, ``istio-init``, in pods
deployed in the mesh. The ``istio-init`` container sets up the pod
network traffic redirection to/from the Istio sidecar proxy. This
requires the user or service-account deploying pods to the mesh to have
sufficient Kubernetes RBAC permissions to deploy `containers with the
``NET_ADMIN`` and ``NET_RAW``
capabilities <https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-capabilities-for-a-container>`_.
Requiring Istio users to have elevated Kubernetes RBAC permissions is
problematic for some organizations’ security compliance. The Istio CNI
plugin is a replacement for the ``istio-init`` container that performs
the same networking functionality but without requiring Istio users to
enable elevated Kubernetes RBAC permissions.

The Istio CNI plugin performs the Istio mesh pod traffic redirection in
the Kubernetes pod lifecycle’s network setup phase, thereby removing the
`requirement for the ``NET_ADMIN`` and ``NET_RAW``
capabilities </docs/ops/deployment/requirements/>`_ for users deploying
pods into the Istio mesh. The Istio CNI plugin replaces the
functionality provided by the ``istio-init`` container.

Prerequisites
-------------

1. Install Kubernetes with the container runtime supporting CNI and
   ``kubelet`` configured with the main
   `CNI <https://github.com/containernetworking/cni>`_ plugin enabled
   via ``--network-plugin=cni``.

   -  AWS EKS, Azure AKS, and IBM Cloud IKS clusters have this capability.
   -  Google Cloud GKE clusters require that the
      `network-policy <https://cloud.google.com/kubernetes-engine/docs/how-to/network-policy>`_
      feature is enabled to have Kubernetes configured with ``network-plugin=cni``.
   -  OpenShift has CNI enabled by default.

2. Install Kubernetes with the `ServiceAccount admission controller <https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#serviceaccount>`_
   enabled.

   -  The Kubernetes documentation highly recommends this for all Kubernetes installations where ``ServiceAccounts`` are utilized.

Installation
------------

1. Determine the Kubernetes environment’s CNI plugin ``--cni-bin-dir``
   and ``--cni-conf-dir`` settings. Refer to `Hosted Kubernetes
   settings <#hosted-kubernetes-settings>`_ for any non-default
   settings required.

2. Install Istio CNI and Istio using ``istioctl``. Refer to the `Istio
   install </docs/setup/install/istioctl/>`_ instructions and pass
   ``--set components.cni.enabled=true`` option. Pass
   ``--set values.cni.cniBinDir=...`` and/or
   ``--set values.cni.cniConfDir=...`` options when installing
   ``istio-cni`` if non-default, as determined in the previous step.

Helm chart parameters
~~~~~~~~~~~~~~~~~~~~~

The following table shows all the options that the ``istio-cni``
configuration supports:

+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
|        Option        |    Values    |     Default      |                                        Description                                        |
+======================+==============+==================+===========================================================================================+
| ``hub``              |              |                  | The container                                                                             |
|                      |              |                  | registry to pull the                                                                      |
|                      |              |                  | ``install-cni``                                                                           |
|                      |              |                  | image.                                                                                    |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``tag``              |              |                  | The container tag to                                                                      |
|                      |              |                  | use to pull the                                                                           |
|                      |              |                  | ``install-cni``                                                                           |
|                      |              |                  | image.                                                                                    |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``pullPolicy         |              | ``Always``       | The image pull policy                                                                     |
| ``                   |              |                  | for the                                                                                   |
|                      |              |                  | ``install-cni``                                                                           |
|                      |              |                  | image.                                                                                    |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``logLevel``         | ``panic``,   | ``warn``         | Logging level for CNI                                                                     |
|                      | ``fatal``,   |                  | binary.                                                                                   |
|                      | ``error``,   |                  |                                                                                           |
|                      | ``warn``,    |                  |                                                                                           |
|                      | ``info``,    |                  |                                                                                           |
|                      | ``debug``    |                  |                                                                                           |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``excludeNam         | ``[]string`` | ``[ istio-syst   | List of namespaces to                                                                     |
| espaces``            |              | em ]``           | exclude from Istio                                                                        |
|                      |              |                  | pod check.                                                                                |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``cniBinDir`         |              | ``/opt/cni/bin`` | Must be the same as                                                                       |
| `                    |              |                  | the environment’s                                                                         |
|                      |              |                  | ``--cni-bin-dir``                                                                         |
|                      |              |                  | setting (``kubelet``                                                                      |
|                      |              |                  | parameter).                                                                               |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``cniConfDir         |              | ``/etc/cni/net   | Must be the same as                                                                       |
| ``                   |              | .d``             | the environment’s                                                                         |
|                      |              |                  | ``--cni-conf-dir``                                                                        |
|                      |              |                  | setting (``kubelet``                                                                      |
|                      |              |                  | parameter).                                                                               |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``cniConfFil         |              |                  | Leave unset to                                                                            |
| eName``              |              |                  | auto-find the first                                                                       |
|                      |              |                  | file in the                                                                               |
|                      |              |                  | ``cni-conf-dir`` (as                                                                      |
|                      |              |                  | ``kubelet`` does).                                                                        |
|                      |              |                  | Primarily used for                                                                        |
|                      |              |                  | testing                                                                                   |
|                      |              |                  | ``install-cni``                                                                           |
|                      |              |                  | plugin configuration.                                                                     |
|                      |              |                  | If set,                                                                                   |
|                      |              |                  | ``install-cni`` will                                                                      |
|                      |              |                  | inject the plugin                                                                         |
|                      |              |                  | configuration into                                                                        |
|                      |              |                  | this file in the                                                                          |
|                      |              |                  | ``cni-conf-dir``.                                                                         |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``psp_cluster_role`` |              |                  | This value refers to                                                                      |
|                      |              |                  | a ``ClusterRole`` and                                                                     |
|                      |              |                  | can be used to create                                                                     |
|                      |              |                  | a ``RoleBinding`` in                                                                      |
|                      |              |                  | the namespace of                                                                          |
|                      |              |                  | ``istio-cni``. This                                                                       |
|                      |              |                  | is useful if you use                                                                      |
|                      |              |                  | `Pod Security Policies <https://kubernetes.io/docs/concepts/policy/pod-security-policy>`_ |
|                      |              |                  | and want to allow                                                                         |
|                      |              |                  | ``istio-cni`` to run                                                                      |
|                      |              |                  | as ``priviliged``                                                                         |
|                      |              |                  | Pods.                                                                                     |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``podAnnotat         |              | ``{}``           | Additional custom                                                                         |
| ions``               |              |                  | annotations to be set                                                                     |
|                      |              |                  | on pod level.                                                                             |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``repair.ena         | ``boolean``  | ``true``         | Enable or disable the                                                                     |
| bled``               |              |                  | `CNI Race                                                                                 |
|                      |              |                  | Condition <https://gi                                                                     |
|                      |              |                  | thub.com/istio/istio/                                                                     |
|                      |              |                  | issues/14327>`_                                                                           |
|                      |              |                  | detection and repair                                                                      |
|                      |              |                  | functionality. This                                                                       |
|                      |              |                  | injects an                                                                                |
|                      |              |                  | ``istio-validation``                                                                      |
|                      |              |                  | init container into                                                                       |
|                      |              |                  | every injected pod,                                                                       |
|                      |              |                  | which checks if Istio                                                                     |
|                      |              |                  | CNI correctly                                                                             |
|                      |              |                  | initialized the pod’s                                                                     |
|                      |              |                  | networking                                                                                |
|                      |              |                  | configuration. It                                                                         |
|                      |              |                  | also enables a new                                                                        |
|                      |              |                  | container in the CNI                                                                      |
|                      |              |                  | ``DaemonSet`` which                                                                       |
|                      |              |                  | monitors for pods and                                                                     |
|                      |              |                  | either labels or                                                                          |
|                      |              |                  | deletes them, per the                                                                     |
|                      |              |                  | values below.                                                                             |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``repair.hub         |              |                  | The container                                                                             |
| ``                   |              |                  | registry to pull the                                                                      |
|                      |              |                  | ``install-cni`` image                                                                     |
|                      |              |                  | for the repair                                                                            |
|                      |              |                  | container. Defaults                                                                       |
|                      |              |                  | to the same as                                                                            |
|                      |              |                  | ``hub``.                                                                                  |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``repair.tag         |              |                  | The container tag to                                                                      |
| ``                   |              |                  | use to pull the                                                                           |
|                      |              |                  | ``install-cni`` image                                                                     |
|                      |              |                  | for the repair                                                                            |
|                      |              |                  | container. Defaults                                                                       |
|                      |              |                  | to the same as                                                                            |
|                      |              |                  | ``tag``.                                                                                  |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``repair.ini         |              | ``istio-valida   | An override for the                                                                       |
| tContainerNa         |              | tion``           | init container name                                                                       |
| me``                 |              |                  | inspected by the                                                                          |
|                      |              |                  | repair controller, if                                                                     |
|                      |              |                  | you are using a                                                                           |
|                      |              |                  | non-standard pod                                                                          |
|                      |              |                  | injection                                                                                 |
|                      |              |                  | configuration.                                                                            |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``repair.lab         | ``boolean``  | ``true``         | Enable the repair                                                                         |
| elPods``             |              |                  | controller to label                                                                       |
|                      |              |                  | pods it detects as                                                                        |
|                      |              |                  | uninitialized.                                                                            |
|                      |              |                  | Ignored if                                                                                |
|                      |              |                  | ``deletePods`` is                                                                         |
|                      |              |                  | true.                                                                                     |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``repair.del         | ``boolean``  | ``true``         | Enable the repair                                                                         |
| etePods``            |              |                  | controller to delete                                                                      |
|                      |              |                  | pods it detects as                                                                        |
|                      |              |                  | uninitialized. It                                                                         |
|                      |              |                  | will continue                                                                             |
|                      |              |                  | deleting those pods                                                                       |
|                      |              |                  | until CNI initializes                                                                     |
|                      |              |                  | them correctly.                                                                           |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``repair.bro         |              | ``cni.istio.io   | The key portion of                                                                        |
| kenPodLabelK         |              | /uninitialized   | the label to add to                                                                       |
| ey``                 |              | ``               | broken pods when                                                                          |
|                      |              |                  | ``labelPods`` is                                                                          |
|                      |              |                  | true.                                                                                     |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``repair.bro         |              | ``true``         | The value portion of                                                                      |
| kenPodLabelV         |              |                  | the label to add to                                                                       |
| alue``               |              |                  | broken pods when                                                                          |
|                      |              |                  | ``labelPods`` is                                                                          |
|                      |              |                  | true.                                                                                     |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+
| ``chained``          | ``true`` or  | ``true``         | Whether to deploy the                                                                     |
|                      | ``false``    |                  | configuration file as                                                                     |
|                      |              |                  | a plugin chain or as                                                                      |
|                      |              |                  | a standalone file in                                                                      |
|                      |              |                  | ``cni-conf-dir``.                                                                         |
|                      |              |                  | Some Kubernetes                                                                           |
|                      |              |                  | flavors                                                                                   |
|                      |              |                  | (e.g. OpenShift) do                                                                       |
|                      |              |                  | not support the chain                                                                     |
|                      |              |                  | approach, set to                                                                          |
|                      |              |                  | ``false`` if this is                                                                      |
|                      |              |                  | the case.                                                                                 |
+----------------------+--------------+------------------+-------------------------------------------------------------------------------------------+

These options are accessed through ``values.cni.<option-name>`` in
``istioctl manifest`` commands, either as a ``--set`` flag, or the
corresponding path in a custom overlay file.

Excluding specific Kubernetes namespaces
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This example uses ``Istioctl`` to perform the following tasks:

-  Install the Istio CNI plugin.
-  Configure its log level.
-  Ignore the pods in the following namespaces:

   -  ``istio-system``
   -  ``foo_ns``
   -  ``bar_ns``

Refer to the `Customizable Install with Istioctl </docs/setup/install/istioctl>`_ for complete instructions.

Use the following command to render and apply Istio CNI components and
override the default configuration of the ``logLevel`` and
``excludeNamespaces`` parameters for ``istio-cni``:

Create a ``IstioControlPlane`` CR yaml locally with your override to
install ``istio``, e.g. \ ``cni.yaml``

.. code:: yaml

    apiVersion: install.istio.io/v1alpha1 kind:
IstioOperator spec: components: cni: enabled: true values: cni:
excludeNamespaces: - istio-system - kube-system - foo_ns - bar_ns
logLevel: info

.. code:: sh

      $ istioctl manifest apply -f cni.yaml

Hosted Kubernetes settings
~~~~~~~~~~~~~~~~~~~~~~~~~~

The Istio CNI solution is not ubiquitous. Some platforms, especially
hosted Kubernetes environments, do not enable the CNI plugin in the
``kubelet`` configuration. The ``istio-cni`` plugin is expected to work
with any hosted Kubernetes leveraging CNI plugins. The following table
shows the required settings for many common Kubernetes environments.

+----------------------------------------------------------+-----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
|                          Hosted                          |                                      Required Istio CNI                                       |                                        Required Platform                                         |
|                       Cluster Type                       |                                       Setting Overrides                                       |                                        Setting Overrides                                         |
+==========================================================+===============================================================================================+==================================================================================================+
| GKE 1.9+(see `GKEsetup <#gke-setup>`_ below for details) | ``--set components.cni.namespace=kube-system --setvalues.cni.cniBinDir=/home/kubernetes/bin`` | enable `network-policy <https://cloud.google.com/kubernetes-engine/docs/how-to/network-policy>`_ |
+----------------------------------------------------------+-----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
| IKS (IBM                                                 | *(none)*                                                                                      | *(none)*                                                                                         |
| cloud)                                                   |                                                                                               |                                                                                                  |
+----------------------------------------------------------+-----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
| EKS (AWS)                                                | *(none)*                                                                                      | *(none)*                                                                                         |
+----------------------------------------------------------+-----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
| AKS (Azure)                                              | *(none)*                                                                                      | *(none)*                                                                                         |
+----------------------------------------------------------+-----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
| Red Hat                                                  | *(none)*                                                                                      | *(none)*                                                                                         |
| OpenShift                                                |                                                                                               |                                                                                                  |
| 3.10+                                                    |                                                                                               |                                                                                                  |
+----------------------------------------------------------+-----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+
| Red Hat                                                  | ``--set components.cni.na                                                                     | *(none)*                                                                                         |
| OpenShift                                                | mespace=kube-system --set                                                                     |                                                                                                  |
| 4.2+                                                     | values.cni.cniBinDir=/va                                                                      |                                                                                                  |
|                                                          | r/lib/cni/bin --set value                                                                     |                                                                                                  |
|                                                          | s.cni.cniConfDir=/etc/cni                                                                     |                                                                                                  |
|                                                          | /multus/net.d --set value                                                                     |                                                                                                  |
|                                                          | s.cni.chained=false --set                                                                     |                                                                                                  |
|                                                          | values.cni.cniConfFileNa                                                                      |                                                                                                  |
|                                                          | me="istio-cni.conf" --set                                                                     |                                                                                                  |
|                                                          | values.sidecarInjectorWe                                                                      |                                                                                                  |
|                                                          | bhook.injectedAnnotations                                                                     |                                                                                                  |
|                                                          | ."k8s\.v1\.cni\.cncf\.io/                                                                     |                                                                                                  |
|                                                          | networks"=istio-cni``                                                                         |                                                                                                  |
+----------------------------------------------------------+-----------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------+

GKE setup
~~~~~~~~~

1. Refer to the procedure to `prepare a GKE cluster for Istio </docs/setup/platform-setup/gke/>`_ and enable
   `network-policy <https://cloud.google.com/kubernetes-engine/docs/how-to/network-policy>`_
   in your cluster.

   .. warning::
      For existing clusters, this redeploys all nodes.

2. Install Istio CNI via ``Istioctl`` including the
   ``--set cniBinDir=/home/kubernetes/bin`` option. For example, the
   following ``istioctl manifest`` command sets the ``cniBinDir`` value
   for a GKE cluster:

   .. code:: sh

      $ istioctl manifest apply –set
   cniBinDir=/home/kubernetes/bin

Sidecar injection compatibility
-------------------------------

The use of the Istio CNI plugin requires Kubernetes pods to be deployed
with a sidecar injection method that uses the ``istio-sidecar-injector``
configmap created from the installation with the
``--set cni.enabled=true`` option. Refer to `Istio sidecar injection </docs/setup/additional-setup/sidecar-injection/>`_ for
details about Istio sidecar injection methods.

The following sidecar injection methods are supported for use with the
Istio CNI plugin:

1. `Automatic sidecar injection </docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection>`_
2. Manual sidecar injection with the ``istio-sidecar-injector`` configmap

   1. `istioctl kube-inject </docs/reference/commands/istioctl/#istioctl-kube-inject>`_ using the configmap directly:

      .. code:: sh

      $ istioctl kube-inject -f deployment.yaml -o
      deployment-injected.yaml –injectConfigMapName
      istio-sidecar-injector $ kubectl apply -f deployment-injected.yaml


   2. ``istioctl kube-inject`` using a file created from the configmap:

      .. code:: sh

      $ kubectl -n istio-system get configmap
      istio-sidecar-injector -o=jsonpath=‘{.data.config}’ >
      inject-config.yaml $ istioctl kube-inject -f deployment.yaml -o
      deployment-injected.yaml –injectConfigFile inject-config.yaml $
      kubectl apply -f deployment-injected.yaml

Operational details
-------------------

The Istio CNI plugin handles Kubernetes pod create and delete events and
does the following:

1. Identify Istio user application pods with Istio sidecars requiring
   traffic redirection
2. Perform pod network namespace configuration to redirect traffic
   to/from the Istio sidecar

Identifying pods requiring traffic redirection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Istio CNI plugin identifies pods requiring traffic redirection
to/from the accompanying Istio proxy sidecar by checking that the pod
meets all of the following conditions:

1. The pod is NOT in a Kubernetes namespace in the configured
   ``exclude_namespaces`` list.
2. The pod has a container named ``istio-proxy``.
3. The pod has more than 1 container.
4. The pod has no annotation with key ``sidecar.istio.io/inject`` OR the
   value of the annotation is ``true``.

Traffic redirection parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To redirect traffic in the application pod’s network namespace to/from
the Istio proxy sidecar, the Istio CNI plugin configures the namespace’s
iptables. The following table describes the parameters to the redirect
functionality. To override the default values for the parameters, set
the corresponding application pod annotation key.

+------------------------+-----------+-------------+-------------------+
| Annotation Key         | Values    | Default     | Description       |
+========================+===========+=============+===================+
| ``sidecar.istio.io/inj | ``true``, | ``true``    | Indicates whether |
| ect``                  | ``false`` |             | the Istio proxy   |
|                        |           |             | sidecar should be |
|                        |           |             | injected. If      |
|                        |           |             | present and       |
|                        |           |             | ``false``, the    |
|                        |           |             | Istio CNI plugin  |
|                        |           |             | doesn’t configure |
|                        |           |             | the namespace’s   |
|                        |           |             | iptables for the  |
|                        |           |             | pod.              |
+------------------------+-----------+-------------+-------------------+
| ``sidecar.istio.io/sta |           |             | Annotation        |
| tus``                  |           |             | created by        |
|                        |           |             | Istio’s sidecar   |
|                        |           |             | injection. If     |
|                        |           |             | missing, the      |
|                        |           |             | Istio CNI plugin  |
|                        |           |             | doesn’t configure |
|                        |           |             | the pod           |
|                        |           |             | namespace’s       |
|                        |           |             | iptables.         |
+------------------------+-----------+-------------+-------------------+
| ``sidecar.istio.io/int | ``REDIREC | ``REDIRECT` | The iptables      |
| erceptionMode``        | T``,      | `           | redirect mode to  |
|                        | ``TPROXY` |             | use.              |
|                        | `         |             |                   |
+------------------------+-----------+-------------+-------------------+
| ``traffic.sidecar.isti | ``<IPCidr | ``"*"``     | Comma separated   |
| o.io/includeOutboundIP | 1>,<IPCid |             | list of IP ranges |
| Ranges``               | r2>,...`` |             | in CIDR form to   |
|                        |           |             | redirect to the   |
|                        |           |             | sidecar proxy.    |
|                        |           |             | The default value |
|                        |           |             | of ``"*"``        |
|                        |           |             | redirects all     |
|                        |           |             | traffic.          |
+------------------------+-----------+-------------+-------------------+
| ``traffic.sidecar.isti | ``<IPCidr |             | Comma separated   |
| o.io/excludeOutboundIP | 1>,<IPCid |             | list of IP ranges |
| Ranges``               | r2>,...`` |             | in CIDR form to   |
|                        |           |             | be excluded from  |
|                        |           |             | redirection. Only |
|                        |           |             | applies when      |
|                        |           |             | ``includeOutbound |
|                        |           |             | IPRanges``        |
|                        |           |             | is ``"*"``.       |
+------------------------+-----------+-------------+-------------------+
| ``traffic.sidecar.isti | ``<port1> | Pod’s list  | Comma separated   |
| o.io/includeInboundPor | ,<port2>, | of          | list of inbound   |
| ts``                   | ...``     | ``container | ports for which   |
|                        |           | Ports``     | traffic is to be  |
|                        |           |             | redirected to the |
|                        |           |             | Istio proxy       |
|                        |           |             | sidecar. The      |
|                        |           |             | value of ``"*"``  |
|                        |           |             | redirects all     |
|                        |           |             | ports.            |
+------------------------+-----------+-------------+-------------------+
| ``traffic.sidecar.isti | ``<port1> |             | Comma separated   |
| o.io/excludeInboundPor | ,<port2>, |             | list of inbound   |
| ts``                   | ...``     |             | ports to be       |
|                        |           |             | excluded from     |
|                        |           |             | redirection to    |
|                        |           |             | the Istio sidecar |
|                        |           |             | proxy. Only valid |
|                        |           |             | when              |
|                        |           |             | ``includeInboundP |
|                        |           |             | orts``            |
|                        |           |             | is ``"*"``.       |
+------------------------+-----------+-------------+-------------------+
| ``traffic.sidecar.isti | ``<port1> |             | Comma separated   |
| o.io/excludeOutboundPo | ,<port2>, |             | list of outbound  |
| rts``                  | ...``     |             | ports to be       |
|                        |           |             | excluded from     |
|                        |           |             | redirection to    |
|                        |           |             | Envoy.            |
+------------------------+-----------+-------------+-------------------+
| ``traffic.sidecar.isti | ``<ethX>, |             | Comma separated   |
| o.io/kubevirtInterface | <ethY>,.. |             | list of virtual   |
| s``                    | .``       |             | interfaces whose  |
|                        |           |             | inbound traffic   |
|                        |           |             | (from VM) will be |
|                        |           |             | treated as        |
|                        |           |             | outbound.         |
+------------------------+-----------+-------------+-------------------+

Logging
~~~~~~~

The Istio CNI plugin runs in the container runtime process space. Due to
this, the ``kubelet`` process writes the plugin’s log entries into its
log.

Compatibility with application init containers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Istio CNI plugin may cause networking connectivity problems for any
application ``initContainers``. When using Istio CNI, ``kubelet`` starts
an injected pod with the following steps:

1. The Istio CNI plugin sets up traffic redirection to the Istio sidecar
   proxy within the pod.
2. All init containers execute and complete successfully.
3. The Istio sidecar proxy starts in the pod along with the pod’s other
   containers.

Init containers execute before the sidecar proxy starts, which can
result in traffic loss during their execution. Avoid this traffic loss
with one or both of the following settings:

-  Set the ``traffic.sidecar.istio.io/excludeOutboundIPRanges``
   annotation to disable redirecting traffic to any CIDRs the init
   containers communicate with.
-  Set the ``traffic.sidecar.istio.io/excludeOutboundPorts`` annotation
   to disable redirecting traffic to the specific outbound ports the
   init containers use.

Compatibility with other CNI plugins
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Istio CNI plugin maintains compatibility with the same set of CNI
plugins as the current ``istio-init`` container which requires the
``NET_ADMIN`` and ``NET_RAW`` capabilities.

The Istio CNI plugin operates as a chained CNI plugin. This means its
configuration is added to the existing CNI plugins configuration as a
new configuration list element. See the `CNI specification
reference <https://github.com/containernetworking/cni/blob/master/SPEC.md#network-configuration-lists>`_
for further details. When a pod is created or deleted, the container
runtime invokes each plugin in the list in order. The Istio CNI plugin
only performs actions to setup the application pod’s traffic redirection
to the injected Istio proxy sidecar (using ``iptables`` in the pod’s
network namespace).

.. warning::

   The Istio CNI plugin should not interfere with the
operations of the base CNI plugin that configures the pod’s networking
setup, although not all CNI plugins have been validated. {{< /warning
>}}
