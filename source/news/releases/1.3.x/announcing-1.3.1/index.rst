announcing-1.3.1
===================

This release includes bug fixes to improve robustness. This release note
describes what’s different between Istio 1.3.0 and Istio 1.3.1.

{{< relnote >}}

Bug fixes
---------

-  **Fixed** an issue which caused the secret cleanup job to erroneously
   run during upgrades (`Issue
   16873 <https://github.com/istio/istio/issues/16873>`_).
-  **Fixed** an issue where the default configuration disabled
   Kubernetes Ingress support (`Issue
   17148 <https://github.com/istio/istio/issues/17148>`_)
-  **Fixed** an issue with handling invalid ``UTF-8`` characters in the
   Stackdriver logging adapter (`Issue
   16966 <https://github.com/istio/istio/issues/16966>`_).
-  **Fixed** an issue which caused the ``destination_service`` label in
   HTTP metrics not to be set for ``BlackHoleCluster`` and
   ``PassThroughCluster`` (`Issue
   16629 <https://github.com/istio/istio/issues/16629>`_).
-  **Fixed** an issue with the ``destination_service`` label in the
   ``istio_tcp_connections_closed_total`` and
   ``istio_tcp_connections_opened_total`` metrics which caused them to
   not be set correctly (`Issue
   17234 <https://github.com/istio/istio/issues/17234>`_).
-  **Fixed** an Envoy crash introduced in Istio 1.2.4 (`Issue
   16357 <https://github.com/istio/istio/issues/16357>`_).
-  **Fixed** Istio CNI sidecar initialization when IPv6 is disabled on
   the node (`Issue
   15895 <https://github.com/istio/istio/issues/15895>`_).
-  **Fixed** a regression affecting support of RS384 and RS512
   algorithms in JWTs (`Issue
   15380 <https://github.com/istio/istio/issues/15380>`_).

Minor enhancements
------------------

-  **Added** support for ``.Values.global.priorityClassName`` to the
   telemetry deployment.
-  **Added** annotations for Datadog tracing that controls extra
   features in sidecars.
-  **Added** the ``pilot_xds_push_time`` metric to report Pilot xDS push
   time.
-  **Added** ``istioctl experimental analyze`` to support multi-resource
   analysis and validation.
-  **Added** support for running metadata exchange and stats extensions
   in a WebAssembly sandbox.
-  **Removed** time diff info in the proxy-status command.
