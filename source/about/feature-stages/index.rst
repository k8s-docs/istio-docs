feature stages
====================================

.. raw:: html

   <!--
   Note: this contains feature status from
   https://docs.google.com/spreadsheets/d/1Nbjat-juyQ8AWhkq3njLckmHM8TRL4O-sjm9Bfr9zrU/edit#gid=0
   -->

This page lists the relative maturity and support level of every Istio
feature. Please note that the phases (Alpha, Beta, and Stable) are
applied to individual features within the project, not to the project as
a whole. Here is a high level description of what these labels mean.

Feature phase definitions
-------------------------

+-----------------+-----------------+-----------------+-----------------+
|                 | Alpha           | Beta            | Stable          |
+=================+=================+=================+=================+
| **Purpose**     | Demo-able,      | Usable in       | Dependable,     |
|                 | works           | production, not | production      |
|                 | end-to-end but  | a toy anymore   | hardened        |
|                 | has             |                 |                 |
|                 | limitations. If |                 |                 |
|                 | you use it in   |                 |                 |
|                 | production and  |                 |                 |
|                 | encounter a     |                 |                 |
|                 | serious issue   |                 |                 |
|                 | we may not be   |                 |                 |
|                 | able to fix it  |                 |                 |
|                 | for you, so be  |                 |                 |
|                 | sure that you   |                 |                 |
|                 | can continue to |                 |                 |
|                 | function if you |                 |                 |
|                 | have to disable |                 |                 |
|                 | it              |                 |                 |
+-----------------+-----------------+-----------------+-----------------+
| **API**         | No guarantees   | APIs are        | Dependable,     |
|                 | on backward     | versioned       | production-wort |
|                 | compatibility   |                 | hy.             |
|                 |                 |                 | APIs are        |
|                 |                 |                 | versioned, with |
|                 |                 |                 | automated       |
|                 |                 |                 | version         |
|                 |                 |                 | conversion for  |
|                 |                 |                 | backward        |
|                 |                 |                 | compatibility   |
+-----------------+-----------------+-----------------+-----------------+
| **Performance** | Not quantified  | Not quantified  | Performance     |
|                 | or guaranteed   | or guaranteed   | (latency/scale) |
|                 |                 |                 | is quantified,  |
|                 |                 |                 | documented,     |
|                 |                 |                 | with guarantees |
|                 |                 |                 | against         |
|                 |                 |                 | regression      |
+-----------------+-----------------+-----------------+-----------------+
| **Deprecation   | None            | Weak - 3 months | Dependable,     |
| Policy**        |                 |                 | Firm. 1 year    |
|                 |                 |                 | notice will be  |
|                 |                 |                 | provided before |
|                 |                 |                 | changes         |
+-----------------+-----------------+-----------------+-----------------+
| **Security**    | Security        | Security        | Security        |
|                 | vulnerabilities | vulnerabilities | vulnerabilities |
|                 | will be handled | will be handled | will be handled |
|                 | publicly as     | according to    | according to    |
|                 | simple bug      | our `security   | our `security   |
|                 | fixes           | vulnerability   | vulnerability   |
|                 |                 | policy </about/ | policy </about/ |
|                 |                 | security-vulner | security-vulner |
|                 |                 | abilities/>`_  | abilities/>`_  |
+-----------------+-----------------+-----------------+-----------------+

Istio features
--------------

Below is our list of existing features and their current phases. This
information will be updated after every monthly release.

Traffic management
~~~~~~~~~~~~~~~~~~

+-----------------------------------+-----------------------------------+
| Feature                           | Phase                             |
+===================================+===================================+
| Protocols: HTTP1.1 / HTTP2 / gRPC | Stable                            |
| / TCP                             |                                   |
+-----------------------------------+-----------------------------------+
| Protocols: Websockets / MongoDB   | Stable                            |
+-----------------------------------+-----------------------------------+
| Traffic Control: label/content    | Stable                            |
| based routing, traffic shifting   |                                   |
+-----------------------------------+-----------------------------------+
| Resilience features: timeouts,    | Stable                            |
| retries, connection pools,        |                                   |
| outlier detection                 |                                   |
+-----------------------------------+-----------------------------------+
| Gateway: Ingress, Egress for all  | Stable                            |
| protocols                         |                                   |
+-----------------------------------+-----------------------------------+
| TLS termination and SNI Support   | Stable                            |
| in Gateways                       |                                   |
+-----------------------------------+-----------------------------------+
| SNI (multiple certs) at ingress   | Stable                            |
+-----------------------------------+-----------------------------------+
| `Locality load                    | Beta                              |
| balancing </docs/ops/configuratio |                                   |
| n/traffic-management/locality-loa |                                   |
| d-balancing/>`_                  |                                   |
+-----------------------------------+-----------------------------------+
| Enabling custom filters in Envoy  | Alpha                             |
+-----------------------------------+-----------------------------------+
| CNI container interface           | Alpha                             |
+-----------------------------------+-----------------------------------+
| `Sidecar                          | Beta                              |
| API </docs/reference/config/netwo |                                   |
| rking/sidecar/>`_                |                                   |
+-----------------------------------+-----------------------------------+

Observability
~~~~~~~~~~~~~

=================================================================================================== ======
Feature                                                                                             Phase
=================================================================================================== ======
`Prometheus Integration </docs/tasks/observability/metrics/querying-metrics/>`_                    Stable
`Local Logging (STDIO) </docs/tasks/observability/mixer/logs/collecting-logs/>`_                   Stable
`Statsd Integration </docs/reference/config/policy-and-telemetry/adapters/statsd/>`_               Stable
`Client and Server Telemetry Reporting </docs/reference/config/policy-and-telemetry/>`_            Stable
`Service Dashboard in Grafana </docs/tasks/observability/metrics/using-istio-dashboard/>`_         Stable
`Istio Component Dashboard in Grafana </docs/tasks/observability/metrics/using-istio-dashboard/>`_ Stable
`Distributed Tracing </docs/tasks/observability/distributed-tracing/>`_                            Stable
`Stackdriver Integration </docs/reference/config/policy-and-telemetry/adapters/stackdriver/>`_     Beta
`Distributed Tracing to Zipkin / Jaeger </docs/tasks/observability/distributed-tracing/>`_         Beta
`Logging with Fluentd </docs/tasks/observability/mixer/logs/fluentd/>`_                            Beta
`Trace Sampling </docs/tasks/observability/distributed-tracing/overview/#trace-sampling>`_         Beta
=================================================================================================== ======

Security and policy enforcement
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

================================================================================================== ======
Feature                                                                                            Phase
================================================================================================== ======
`Deny Checker </docs/reference/config/policy-and-telemetry/adapters/denier/>`_                    Stable
`List Checker </docs/reference/config/policy-and-telemetry/adapters/list/>`_                      Stable
`Pluggable Key/Cert Support for Istio CA </docs/tasks/security/citadel-config/plugin-ca-cert/>`_  Stable
`Service-to-service mutual TLS </docs/concepts/security/#mutual-tls-authentication>`_             Stable
`Kubernetes: Service Credential Distribution </docs/concepts/security/#pki>`_                     Stable
`VM: Service Credential Distribution </docs/concepts/security/#pki>`_                             Beta
`Mutual TLS Migration </docs/tasks/security/authentication/mtls-migration>`_                      Beta
`Cert management on Ingress Gateway </docs/tasks/traffic-management/ingress/secure-ingress-sds>`_ Beta
`Authorization </docs/concepts/security/#authorization>`_                                         Beta
`End User (JWT) Authentication </docs/concepts/security/#authentication>`_                        Alpha
`OPA Checker </docs/reference/config/policy-and-telemetry/adapters/opa/>`_                        Alpha
`SDS Integration </docs/tasks/security/citadel-config/auth-sds/>`_                                Alpha
================================================================================================== ======

Core
~~~~

============================================================================================================================== ==========
Feature                                                                                                                        Phase
============================================================================================================================== ==========
`Standalone Operator </docs/setup/install/standalone-operator/>`_                                                             Alpha
`Kubernetes: Envoy Installation and Traffic Interception </docs/setup/>`_                                                     Stable
`Kubernetes: Istio Control Plane Installation </docs/setup/>`_                                                                Stable
`Attribute Expression Language </docs/reference/config/policy-and-telemetry/expression-language/>`_                           Stable
Mixer Out-of-Process Adapter Authoring Model                                                                                   Beta
`Helm </docs/setup/install/helm/>`_                                                                                           Beta
`Multicluster Mesh over VPN </docs/setup/install/multicluster/>`_                                                             Alpha
`Kubernetes: Istio Control Plane Upgrade </docs/setup/>`_                                                                     Beta
Consul Integration                                                                                                             Alpha
Basic Configuration Resource Validation                                                                                        Beta
Configuration Processing with Galley                                                                                           Beta
`Mixer Self Monitoring </faq/mixer/#mixer-self-monitoring>`_                                                                  Beta
`Custom Mixer Build Model <https://github.com/istio/istio/wiki/Mixer-Compiled-In-Adapter-Dev-Guide>`_                         deprecated
`Out of Process Mixer Adapters (gRPC Adapters) <https://github.com/istio/istio/wiki/Mixer-Out-Of-Process-Adapter-Dev-Guide>`_ Beta
`Istio CNI plugin </docs/setup/additional-setup/cni/>`_                                                                       Alpha
IPv6 support for Kubernetes                                                                                                    Alpha
`Distroless base images for Istio </docs/ops/configuration/security/harden-docker-images/>`_                                  Alpha
============================================================================================================================== ==========

.. note::

   Please get in touch by joining our
`community </about/community/>`_ if there are features you’d like to
see in our future releases!
