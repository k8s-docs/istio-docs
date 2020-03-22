announcing-1.1.10
=========================

We’re pleased to announce the availability of Istio 1.1.10. Please see
below for what’s changed.

{{< relnote >}}

Bug fixes
---------

-  Eliminate 503 errors caused by Envoy not being able to talk to the
   SDS Node Agent after a restart (`Issue
   14853 <https://github.com/istio/istio/issues/14853>`_).
-  Fix cause of ‘TLS error: Secret is not supplied by SDS’ errors during
   upgrade (`Issue
   15020 <https://github.com/istio/istio/issues/15020>`_).
-  Fix crash in Istio’s JWT Envoy filter caused by malformed JWT (`Issue
   15084 <https://github.com/istio/istio/issues/15084>`_).
