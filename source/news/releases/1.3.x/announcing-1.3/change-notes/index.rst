change-notes
==================

Installation
------------

-  **Added** experimental `manifest and profile
   commands </docs/setup/install/istioctl/>`_ to install and manage the
   Istio control plane for evaluation.

Traffic management
------------------

-  **Added** `automatic protocol
   determination </docs/ops/configuration/traffic-management/protocol-selection/>`_
   of HTTP or TCP for outbound traffic when ports are not named
   according to Istio’s
   `conventions </docs/ops/deployment/requirements/>`_.
-  **Added** a mode to the Gateway API for mutual TLS operation.
-  **Fixed** issues present when a service communicates over the network
   first in permissive mutual TLS mode for protocols like MySQL and
   MongoDB.
-  **Improved** Envoy proxy readiness checks. They now check Envoy’s
   readiness status.
-  **Improved** container ports are no longer required in the pod spec.
   All ports are `captured by
   default </faq/traffic-management/#controlling-inbound-ports>`_.
-  **Improved** the ``EnvoyFilter`` API. You can now add or update all
   configurations.
-  **Improved** the Redis load balancer to now default to
   `MAGLEV <https://www.envoyproxy.io/docs/envoy/v1.6.0/intro/arch_overview/load_balancing#maglev>`_
   when using the Redis proxy.
-  **Improved** load balancing to direct traffic to the `same region and
   zone </faq/traffic-management/#controlling-inbound-ports>`_ by
   default.
-  **Improved** Pilot by reducing CPU utilization. The reduction
   approaches 90% depending on the specific deployment.
-  **Improved** the ``ServiceEntry`` API to allow for the same hostname
   in different namespaces.
-  **Improved** the `Sidecar
   API </docs/reference/config/networking/sidecar/#OutboundTrafficPolicy>`_
   to customize the ``OutboundTrafficPolicy`` policy.

Security
--------

-  **Added** trust domain validation for services using mutual TLS. By
   default, the server only authenticates the requests from the same
   trust domain.
-  **Added** [labels]((/docs/ops/configuration/mesh/secret-creation/) to
   control service account secret generation by namespace.
-  **Added** SDS support to deliver the private key and certificates to
   each Istio control plane service.
-  **Added** support for
   `introspection </docs/ops/diagnostic-tools/controlz/>`_ to Citadel.
-  **Added** metrics to the ``/metrics`` endpoint of Citadel Agent on
   port 15014 to monitor the SDS service.
-  **Added** diagnostics to the Citadel Agent using the
   ``/debug/sds/workload`` and ``/debug/sds/gateway`` on port 8080.
-  **Improved** the ingress gateway to `load the trusted CA certificate
   from a separate
   secret </docs/tasks/traffic-management/ingress/secure-ingress-sds/#configure-a-mutual-tls-ingress-gateway>`_
   when using SDS.
-  **Improved** SDS security by enforcing the usage of `Kubernetes
   Trustworthy JWTs </blog/2019/trustworthy-jwt-sds>`_.
-  **Improved** Citadel Agent logs by unifying the logging pattern.
-  **Removed** support for Istio SDS when using `Kubernetes versions
   earlier than 1.13 </blog/2019/trustworthy-jwt-sds>`_.
-  **Removed** integration with Vault CA temporarily. SDS requirements
   caused the temporary removal but we will reintroduce Vault CA
   integration in a future release.
-  **Enabled** the Envoy JWT filter by default to improve security and
   reliability.

Telemetry
---------

-  **Added** Access Log Service
   `ALS <https://www.envoyproxy.io/docs/envoy/latest/api-v2/service/accesslog/v2/als.proto#grpc-access-log-service-als>`_
   support for Envoy gRPC.
-  **Added** a Grafana dashboard for Citadel monitoring.
-  **Added**
   `metrics </docs/reference/commands/sidecar-injector/#metrics>`_ for
   monitoring the sidecar injector webhook.
-  **Added** control plane metrics to monitor Istio’s configuration
   state.
-  **Added** telemetry reporting for traffic destined to the
   ``Passthrough`` and ``BlackHole`` clusters.
-  **Added** alpha support for in-proxy generation of service metrics
   using Prometheus.
-  **Added** alpha support for environmental metadata in Envoy node
   metadata.
-  **Added** alpha support for Proxy Metadata Exchange.
-  **Added** alpha support for the OpenCensus trace driver.
-  **Improved** reporting for external services by removing requirements
   to add a service entry.
-  **Improved** the mesh dashboard to provide monitoring of Istio’s
   configuration state.
-  **Improved** the Pilot dashboard to expose additional key metrics to
   more clearly identify errors.
-  **Removed** deprecated ``Adapter`` and ``Template`` custom resource
   definitions (CRDs).
-  **Deprecated** the HTTP API spec used to produce API attributes. We
   will remove support for producing API attributes in Istio 1.4.

Policy
------

-  **Improved** rate limit enforcement to allow communication when the
   quota backend is unavailable.

Configuration management
------------------------

-  **Fixed** Galley to stop too many gRPC pings from closing
   connections.
-  **Improved** Galley to avoid control plane upgrade failures.

``istioctl``
------------

-  **Added**
   `istioctl experimental manifest </docs/reference/commands/istioctl/#istioctl-manifest>`_
   to manage the new experimental install manifests.
-  **Added**
   `istioctl experimental profile </docs/reference/commands/istioctl/#istioctl-profile>`_
   to manage the new experimental install profiles.
-  **Added**
   `istioctl experimental metrics </docs/reference/commands/istioctl/#istioctl-experimental-metrics>`_
-  **Added**
   `istioctl experimental describe pod </docs/reference/commands/istioctl/#istioctl-experimental-describe-pod>`_
   to describe an Istio pod’s configuration.
-  **Added**
   `istioctl experimental add-to-mesh </docs/reference/commands/istioctl/#istioctl-experimental-add-to-mesh>`_
   to add Kubernetes services or virtual machines to an existing Istio
   service mesh.
-  **Added**
   `istioctl experimental remove-from-mesh </docs/reference/commands/istioctl/#istioctl-experimental-remove-from-mesh>`_
   to remove Kubernetes services or virtual machines from an existing
   Istio service mesh.
-  **Promoted** the
   `istioctl experimental convert-ingress </docs/reference/commands/istioctl/#istioctl-convert-ingress>`_
   command to ``istioctl convert-ingress``.
-  **Promoted** the
   `istioctl experimental dashboard </docs/reference/commands/istioctl/#istioctl-dashboard>`_
   command to ``istioctl dashboard``.

Miscellaneous
-------------

-  **Added** new images based on
   `distroless </docs/ops/configuration/security/harden-docker-images/>`_
   base images.
-  **Improved** the Istio CNI Helm chart to have consistent versions
   with Istio.
-  **Improved** Kubernetes Jobs behavior. Kubernetes Jobs now exit
   correctly when the job manually calls the ``/quitquitquit`` endpoint.
