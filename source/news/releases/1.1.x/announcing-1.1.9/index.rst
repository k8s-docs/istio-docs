announcing-1.1.9
=========================

We’re pleased to announce the availability of Istio 1.1.9. Please see
below for what’s changed.

{{< relnote >}}

Bug fixes
---------

-  Prevent overly large strings from being sent to Prometheus (`Issue
   14642 <https://github.com/istio/istio/issues/14642>`_).
-  Reuse previously cached JWT public keys if transport errors are
   encountered during renewal (`Issue
   14638 <https://github.com/istio/istio/issues/14638>`_).
-  Bypass JWT authentication for HTTP OPTIONS methods to support CORS
   requests.
-  Fix Envoy crash caused by the Mixer filter (`Issue
   14707 <https://github.com/istio/istio/issues/14707>`_).

Small enhancements
------------------

-  Expose cryptographic signature verification functions to ``Lua``
   Envoy filters (`Envoy Issue
   7009 <https://github.com/envoyproxy/envoy/issues/7009>`_).
