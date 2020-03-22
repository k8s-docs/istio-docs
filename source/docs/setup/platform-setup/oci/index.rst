Oracle Cloud Infrastructure
==============================

Follow these instructions to prepare an OKE cluster for Istio.

1. Create a new OKE cluster within your OCI tenancy. The simplest way to
   do this is by using the ‘Quick Cluster’ option within the `web
   console <https://docs.cloud.oracle.com/iaas/Content/ContEng/Tasks/contengcreatingclusterusingoke.htm>`_.
   You may also use the `OCI
   cli <https://docs.cloud.oracle.com/iaas/Content/API/SDKDocs/cliinstall.htm>`_
   as shown below.

   | .. code:: sh

      $ oci ce cluster create –name oke-cluster1
   | –kubernetes-version
   | –vcn-id
   | –service-lb-subnet-ids []
   | ..

2. Retrieve your credentials for ``kubectl`` using the OCI cli.

   | .. code:: sh

      $ oci ce cluster create-kubeconfig
   | –file
   | –cluster-id

3. Grant cluster administrator (admin) permissions to the current user.
   To create the necessary RBAC rules for Istio, the current user
   requires admin permissions.

   | .. code:: sh

      $ kubectl create clusterrolebinding
     cluster-admin-binding
   | –clusterrole=cluster-admin
   | –user=
