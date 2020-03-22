non-istio-to-istio
=======================

Non-Istio services cannot communicate to Istio services unless they can
present a valid certificate, which is less likely to happen. This is the
expected behavior for *mutual TLS*. However, you can override the global
flag for specific namespaces or services. See
`task </docs/tasks/security/authentication/authn-policy>`_ for more
details.
