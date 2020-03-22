how-distributed-tracing-works
==================================

Istio integrates with distributed tracing systems in two different ways:
`Envoy-based <#how-envoy-based-tracing-works>`_ and
`Mixer-based <#how-mixer-based-tracing-works>`_ tracing integrations.
For both tracing integration approaches, `applications are responsible
for forwarding tracing headers <#istio-copy-headers>`_ for subsequent
outgoing requests.

You can find additional information in the Istio Distributed Tracing
(`Jaeger </docs/tasks/observability/distributed-tracing/jaeger/>`_,
`LightStep </docs/tasks/observability/distributed-tracing/lightstep/>`_,
`Zipkin </docs/tasks/observability/distributed-tracing/zipkin/>`_)
Tasks and in the `Envoy tracing
docs <https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/observability/tracing>`_.
