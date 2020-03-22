announcing-1.3.3
==========================

This release includes bug fixes to improve robustness. This release note
describes what’s different between Istio 1.3.2 and Istio 1.3.3.

{{< relnote >}}

Bug fixes
---------

-  **Fixed** an issue which caused Prometheus to install improperly when
   using ``istioctl x manifest apply``. (`Issue
   16970 <https://github.com/istio/istio/issues/16970>`_)
-  **Fixed** a bug where locality load balancing can not read locality
   information from the node. (`Issue
   17337 <https://github.com/istio/istio/issues/17337>`_)
-  **Fixed** a bug where long-lived connections were getting dropped by
   the Envoy proxy as the listeners were getting reconfigured without
   any user configuration changes. (`Issue
   17383 <https://github.com/istio/istio/issues/17383>`_, `Issue
   17139 <https://github.com/istio/istio/issues/17139>`_)
-  **Fixed** a crash in ``istioctl x analyze`` command. (`Issue
   17449 <https://github.com/istio/istio/issues/17449>`_)
-  **Fixed** ``istioctl x manifest diff`` to diff text blocks in
   ConfigMaps. (`Issue
   16828 <https://github.com/istio/istio/issues/16828>`_)
-  **Fixed** a segmentation fault crash in the Envoy proxy. (`Issue
   17699 <https://github.com/istio/istio/issues/17699>`_)
