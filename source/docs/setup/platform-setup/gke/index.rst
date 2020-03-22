Google kubectl Engine
============================

Follow these instructions to prepare a GKE cluster for Istio.

1. Create a new cluster.

   | .. code:: sh

      $ gcloud container clusters create
   | –cluster-version latest
   | –machine-type=n1-standard-2
   | –num-nodes 4
   | –zone
   | –project

   .. note::

   The default installation of Istio requires nodes with >1
   vCPU. If you are installing with the `demo configuration
   profile </docs/setup/additional-setup/config-profiles/>`_, you can
   remove the ``--machine-type`` argument to use the smaller
   ``n1-standard-1`` machine size instead.

   .. warning::

   To use the Istio CNI feature, the
   `network-policy <https://cloud.google.com/kubernetes-engine/docs/how-to/network-policy>`_
   GKE feature must be enabled in the cluster. Use the
   ``--enable-network-policy`` flag in the
   ``gcloud container clusters create`` command.

2. Retrieve your credentials for ``kubectl``.

   | .. code:: sh

      $ gcloud container clusters get-credentials
   | –zone
   | –project

3. Grant cluster administrator (admin) permissions to the current user.
   To create the necessary RBAC rules for Istio, the current user
   requires admin permissions.

   | .. code:: sh

      $ kubectl create clusterrolebinding
     cluster-admin-binding
   | –clusterrole=cluster-admin
   | –user=$(gcloud config get-value core/account)
