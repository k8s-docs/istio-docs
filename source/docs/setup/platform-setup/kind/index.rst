Kind
============================

`kind <https://kind.sigs.k8s.io/>`_ is a tool for running local
Kubernetes clusters using Docker container ``nodes``. kind was primarily
designed for testing Kubernetes itself, but may be used for local
development or CI. Follow these instructions to prepare a kind cluster
for Istio installation.

Prerequisites
-------------

-  Please use the latest Go version, ideally Go 1.13 or greater.
-  To use kind, you will also need to `install
   docker <https://docs.docker.com/install/>`_.
-  Install the latest version of
   `kind <https://kind.sigs.k8s.io/docs/user/quick-start/>`_.

Installation steps
------------------

1. Create a cluster with the following command:

   .. code:: sh

      $ kind create cluster –name istio-testing {{< /text
   >}}

   ``--name`` is used to assign a specific name to the cluster. By
   default, the cluster will be given the name ``kind``.

2. To see the list of kind clusters, use the following command:

   .. code:: sh

      $ kind get clusters istio-testing

3. To list the local Kubernetes contexts, use the following command.

   .. code:: sh

      $ kubectl config get-contexts CURRENT NAME CLUSTER
   AUTHINFO NAMESPACE

   -  ::

             kind-istio-testing   kind-istio-testing   kind-istio-testing
             minikube             minikube             minikube



   .. note::

   ``kind`` is prefixed to the context and cluster names,
   for example: ``kind-istio-testing``

4. If you run multiple clusters, you need to choose which cluster
   ``kubectl`` talks to. You can set a default cluster for ``kubectl``
   by setting the current context in the `Kubernetes
   kubeconfig <https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/>`_
   file. Additionally you can run following command to set the current
   context for ``kubectl``.

   .. code:: sh

      $ kubectl config use-context kind-istio-testing
   Switched to context “kind-istio-testing”.

   Once you are done setting up a kind cluster, you can proceed to
   `install Istio </docs/setup/getting-started/#download>`_ on it.

5. When you are done experimenting and you want to delete the existing
   cluster, use the following command:

   .. code:: sh

      $ kind delete cluster –name istio-testing Deleting
   cluster “istio-testing” …

Setup Dashboard UI for kind
---------------------------

kind does not have a built in Dashboard UI like minikube. But you can
still setup Dashboard, a web based Kubernetes UI, to view your cluster.
Follow these instructions to setup Dashboard for kind.

1. To deploy Dashboard, run the following command:

   .. code:: sh

      $ kubectl apply -f
   https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml


2. Verify that Dashboard is deployed and running.

   .. code:: sh

      $ kubectl get pod -n kubernetes-dashboard NAME
   READY STATUS RESTARTS AGE dashboard-metrics-scraper-76585494d8-zdb66
   1/1 Running 0 39s kubernetes-dashboard-b7ffbc8cb-zl8zg 1/1 Running 0
   39s

3. Create a ``ClusterRoleBinding`` to provide admin access to the newly
   created cluster.

   .. code:: sh

      $ kubectl create clusterrolebinding default-admin
   –clusterrole cluster-admin –serviceaccount=default:default {{< /text
   >}}

4. To login to Dashboard, you need a Bearer Token. Use the following
   command to store the token in a variable.

   .. code:: sh

      $ token=$(kubectl get secrets -o
   jsonpath=“{.items[?(@.metadata.annotations[‘kubernetes.io/service-account.name’]==‘default’)].data.token}”\|base64
   -d)

   Display the token using the ``echo`` command and copy it to use for
   logging into Dashboard.

   .. code:: sh

      $ echo $token

5. You can Access Dashboard using the kubectl command-line tool by
   running the following command:

   .. code:: sh

      $ kubectl proxy Starting to serve on 127.0.0.1:8001


   Click `Kubernetes
   Dashboard <http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/>`_
   to view your deployments and services.

   .. warning::

   You have to save your token somewhere, otherwise you
   have to run step number 4 everytime you need a token to login to your
   Dashboard.
