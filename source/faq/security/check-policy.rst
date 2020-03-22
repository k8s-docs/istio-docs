check-policy
===============

The `istioctl </docs/reference/commands/istioctl>`_ command
provides an option for this purpose. You can do:

.. code:: sh

      $ istioctl authn tls-check $CLIENT_POD
httpbin.default.svc.cluster.local HOST:PORT STATUS SERVER CLIENT AUTHN
POLICY DESTINATION RULE httpbin.default.svc.cluster.local:8000 OK STRICT
ISTIO_MUTUAL /default istio-system/default

Where ``$CLIENT_POD`` is the ID of one of the client service’s pods.
