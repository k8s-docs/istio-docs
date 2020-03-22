Azure
============================

Follow these instructions to prepare an Azure cluster for Istio.

You can deploy a Kubernetes cluster to Azure via
`AKS <https://azure.microsoft.com/en-us/services/kubernetes-service/>`_
or `AKS-Engine <https://github.com/azure/aks-engine>`_ which fully
supports Istio.

AKS
---

You can create an AKS cluster via `the az
cli <https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough>`_
or `the Azure
portal <https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough-portal>`_.

For the ``az`` cli option, complete ``az login`` authentication OR use
cloud shell, then run the following commands below.

1. Determine the desired region name which supports AKS

   .. code:: sh

      $ az provider list –query
   “[?namespace==‘Microsoft.ContainerService’].resourceTypes[] \|
   [?resourceType==‘managedClusters’].locations[]” -o tsv

2. Verify the supported Kubernetes versions for the desired region

   Replace ``my location`` using the desired region value from the above
   step, and then execute:

   .. code:: sh

      $ az aks get-versions –location “my location”
   –query “orchestrators[].orchestratorVersion”

   Ensure a minimum of ``1.10.5`` is listed.

3. Create the resource group and deploy the AKS cluster

   Replace ``myResourceGroup`` and ``myAKSCluster`` with desired names,
   ``my location`` using the value from step 1, ``1.10.5`` if not
   supported in the region, and then execute:

   .. code:: sh

      $ az group create –name myResourceGroup –location
   “my location” $ az aks create –resource-group myResourceGroup –name
   myAKSCluster –node-count 3 –kubernetes-version 1.10.5
   –generate-ssh-keys

4. Get the AKS ``kubeconfig`` credentials

   Replace ``myResourceGroup`` and ``myAKSCluster`` with the names from
   the previous step and execute:

   .. code:: sh

      $ az aks get-credentials –resource-group
   myResourceGroup –name myAKSCluster

AKS-Engine
----------

1. `Follow the
   instructions <https://github.com/Azure/aks-engine/blob/master/docs/tutorials/quickstart.md#install-aks-engine>`_
   to get and install the ``aks-engine`` binary.

2. Download the ``aks-engine`` API model definition that supports
   deploying Istio:

   .. code:: sh

      $ wget
   https://raw.githubusercontent.com/Azure/aks-engine/master/examples/service-mesh/istio.json


   Note: It is possible to use other api model definitions which will
   work with Istio. The MutatingAdmissionWebhook and
   ValidatingAdmissionWebhook admission control flags and RBAC are
   enabled by default. See `aks-engine api model default
   values <https://github.com/Azure/aks-engine/blob/master/docs/topics/clusterdefinitions.md>`_
   for further information.

3. Deploy your cluster using the ``istio.json`` template. You can find
   references to the parameters in the `official
   docs <https://github.com/Azure/aks-engine/blob/master/docs/tutorials/deploy.md#step-3-edit-your-cluster-definition>`_.

   =================== =====================
   Parameter           Expected value
   =================== =====================
   ``subscription_id`` Azure Subscription Id
   ``dns_prefix``      Cluster DNS Prefix
   ``location``        Cluster Location
   =================== =====================

   | .. code:: sh

      $ aks-engine deploy –subscription-id
   | –dns-prefix –location –auto-suffix
   | –api-model istio.json

   .. note::

   After a few minutes, you can find your cluster on your
   Azure subscription in a resource group called ``<dns_prefix>-<id>``.
   Assuming ``dns_prefix`` has the value ``myclustername``, a valid
   resource group with a unique cluster ID is ``mycluster-5adfba82``.
   The ``aks-engine`` generates your ``kubeconfig`` file in the
   ``_output`` folder.

4. Use the ``<dns_prefix>-<id>`` cluster ID, to copy your ``kubeconfig``
   to your machine from the ``_output`` folder:

   | .. code:: sh

      $ cp \_output/-/kubeconfig/kubeconfig..json
   | ~/.kube/config

   For example:

   | .. code:: sh

      $ cp
     \_output/mycluster-5adfba82/kubeconfig/kubeconfig.westus2.json
   | ~/.kube/config
