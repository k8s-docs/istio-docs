announcing-1.1.1
=========================

We’re pleased to announce the availability of Istio 1.1.1. Please see
below for what’s changed.

{{< relnote >}}

Bug fixes and minor enhancements
--------------------------------

-  Configure Prometheus to monitor Citadel (`Issue
   12175 <https://github.com/istio/istio/pull/12175>`_)
-  Improve output of
   `istioctl verify-install </docs/reference/commands/istioctl/#istioctl-verify-install>`_
   command (`Issue 12174 <https://github.com/istio/istio/pull/12174>`_)
-  Reduce log level for missing service account messages for a SPIFFE
   URI (`Issue 12108 <https://github.com/istio/istio/issues/12108>`_)
-  Fix broken path on the opt-in SDS feature’s Unix domain socket
   (`Issue 12688 <https://github.com/istio/istio/pull/12688>`_)
-  Fix Envoy tracing that was preventing a child span from being created
   if the parent span was propagated with an empty string (`Envoy Issue
   6263 <https://github.com/envoyproxy/envoy/pull/6263>`_)
-  Add namespace scoping to the Gateway ‘port’ names. This fixes two
   issues:

   -  ``IngressGateway`` only respects first port 443 Gateway definition
      (`Issue 11509 <https://github.com/istio/istio/issues/11509>`_)
   -  Istio ``IngressGateway`` routing broken with two different
      gateways with same port name (SDS) (`Issue
      12500 <https://github.com/istio/istio/issues/12500>`_)

-  Five bug fixes for locality weighted load balancing:

   -  Fix bug causing empty endpoints per locality (`Issue
      12610 <https://github.com/istio/istio/issues/12610>`_)
   -  Apply locality weighted load balancing configuration correctly
      (`Issue 12587 <https://github.com/istio/istio/issues/12587>`_)
   -  Locality label ``istio-locality`` in Kubernetes should not contain
      ``/``, use ``.`` (`Issue
      12582 <https://github.com/istio/istio/issues/12582>`_)
   -  Fix crash in locality load balancing (`Issue
      12649 <https://github.com/istio/istio/pull/12649>`_)
   -  Fix bug in locality load balancing normalization (`Issue
      12579 <https://github.com/istio/istio/pull/12579>`_)

-  Propagate Envoy Metrics Service configuration (`Issue
   12569 <https://github.com/istio/istio/issues/12569>`_)
-  Do not apply ``VirtualService`` rule to the wrong gateway (`Issue
   10313 <https://github.com/istio/istio/issues/10313>`_)
