before-you-begin-egress
=================================

Before you begin
----------------

-  Setup Istio by following the instructions in the `Installation
   guide </docs/setup/>`_.

-  Deploy the
   `sleep <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/sleep>`_ sample
   app to use as a test source for sending requests. If you have
   `automatic sidecar
   injection </docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection>`_
   enabled, run the following command to deploy the sample app:

   .. code:: sh

      $ kubectl apply -f @samples/sleep/sleep.yaml@

   Otherwise, manually inject the sidecar before deploying the ``sleep``
   application with the following command:

   .. code:: sh

      $ kubectl apply -f <(istioctl kube-inject -f
   @samples/sleep/sleep.yaml@)

   .. note::

   You can use any pod with ``curl`` installed as a test
   source.

-  Set the ``SOURCE_POD`` environment variable to the name of your
   source pod:

   .. code:: sh

      $ export SOURCE_POD=$(kubectl get pod -l app=sleep
   -o jsonpath={.items..metadata.name})
