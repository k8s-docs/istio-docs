announcing-1.1.12
=========================

We’re pleased to announce the availability of Istio 1.1.12. Please see
below for what’s changed.

{{< relnote >}}

Bug fixes
---------

-  Fix a bug where the sidecar could infinitely forward requests to
   itself when a ``Pod`` resource defines a port that isn’t defined for
   a service (`Issue
   14443 <https://github.com/istio/istio/issues/14443>`_) and (`Issue
   14242 <https://github.com/istio/istio/issues/14242>`_)
