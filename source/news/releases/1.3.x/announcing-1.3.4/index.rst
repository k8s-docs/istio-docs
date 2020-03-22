announcing-1.3.4
==========================

This release includes bug fixes to improve robustness. This release note
describes what’s different between Istio 1.3.3 and Istio 1.3.4.

{{< relnote >}}

Bug fixes
---------

-  **Fixed** a crashing bug in the Google node agent provider. (`Pull
   Request #18296 <https://github.com/istio/istio/pull/18260>`_)
-  **Fixed** Prometheus annotations and updated Jaeger to 1.14. (`Pull
   Request #18274 <https://github.com/istio/istio/pull/18274>`_)
-  **Fixed** in-bound listener reloads that occur on 5 minute intervals.
   (`Issue #18138 <https://github.com/istio/istio/issues/18088>`_)
-  **Fixed** validation of key and certificate rotation. (`Issue
   #17718 <https://github.com/istio/istio/issues/17718>`_)
-  **Fixed** invalid internal resource garbage collection. (`Issue
   #16818 <https://github.com/istio/istio/issues/16818>`_)
-  **Fixed** webhooks that were not updated on a failure. (`Pull Request
   #17820 <https://github.com/istio/istio/pull/17820>`_
-  **Improved** performance of OpenCensus tracing adapter. (`Issue
   #18042 <https://github.com/istio/istio/issues/18042>`_)

Minor enhancements
------------------

-  **Improved** reliability of the SDS service. (`Issue
   #17409 <https://github.com/istio/istio/issues/17409>`_, `Issue
   #17905 <https://github.com/istio/istio/issues/17905>`_)
-  **Added** stable versions of failure domain labels. (`Pull Request
   #17755 <https://github.com/istio/istio/pull/17755>`_)
-  **Added** update of the global mesh policy on upgrades. (`Pull
   Request #17033 <https://github.com/istio/istio/pull/17033>`_)
