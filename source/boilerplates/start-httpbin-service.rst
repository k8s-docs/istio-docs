start-httpbin-service
=================================

-  Start the
   `httpbin <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/httpbin>`_
   sample.

   If you have enabled `automatic sidecar
   injection </docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection>`_,
   deploy the ``httpbin`` service:

   .. code:: sh

      $ kubectl apply -f @samples/httpbin/httpbin.yaml@


   Otherwise, you have to manually inject the sidecar before deploying
   the ``httpbin`` application:

   .. code:: sh

      $ kubectl apply -f <(istioctl kube-inject -f
   @samples/httpbin/httpbin.yaml@)
