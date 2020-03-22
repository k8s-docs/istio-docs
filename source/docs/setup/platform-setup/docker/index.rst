Docker Desktop
============================

1. To run Istio with Docker Desktop, install a version which contains a
   supported Kubernetes version ({{< supported_kubernetes_versions >}}).

2. If you want to run Istio under Docker Desktop’s built-in Kubernetes,
   you need to increase Docker’s memory limit under the *Advanced* pane
   of Docker Desktop’s preferences. Set the resources to 8.0 ``GB`` of
   memory and 4 ``CPUs``.

   .. image::./dockerprefs.png
      :alt:
      :caption:Docker Preferences
      :width: 80%

   .. warning::

   Minimum memory requirements vary. 8 ``GB`` is
   sufficent to run Istio and Bookinfo. If you don’t have enough memory
   allocated in Docker Desktop, the following errors could occur:

   -  image pull failures
   -  healthcheck timeout failures
   -  kubectl failures on the host
   -  general network instability of the hypervisor

   Additional Docker Desktop resources may be freed up using:

   .. code:: sh

      $ docker system prune


