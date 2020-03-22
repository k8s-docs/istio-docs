MicroK8s
============================

Follow these instructions to prepare MicroK8s for using Istio.

.. warning::

   Administrative privileges are required to run MicroK8s.


1. Install the latest version of `MicroK8s <https://microk8s.io>`_
   using the command

   .. code:: sh

      $ sudo snap install microk8s –classic

2. Enable Istio with the following command:

   .. code:: sh

      $ microk8s.enable istio

3. When prompted, choose whether to enforce mutual TLS authentication
   among sidecars. If you have a mixed deployment with non-Istio and
   Istio enabled services or you’re unsure, choose No.

Please run the following command to check deployment progress:

::

   .. code:: sh


   $ watch microk8s.kubectl get all --all-namespaces

