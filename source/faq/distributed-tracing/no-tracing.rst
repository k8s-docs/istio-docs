no-tracing
==================================

Since Istio 1.0.3, the sampling rate for tracing has been reduced to 1%
in the ``default`` `configuration
profile </docs/setup/additional-setup/config-profiles/>`_. This means
that only 1 out of 100 trace instances captured by Istio will be
reported to the tracing backend. The sampling rate in the ``demo``
profile is still set to 100%. See `this
section </docs/tasks/observability/distributed-tracing/overview/#trace-sampling>`_
for more information on how to set the sampling rate.

If you still do not see any trace data, please confirm that your ports
conform to the Istio `port naming
conventions </faq/traffic-management/#naming-port-convention>`_ and
that the appropriate container port is exposed (via pod spec, for
example) to enable traffic capture by the sidecar proxy (Envoy).

If you only see trace data associated with the egress proxy, but not the
ingress proxy, it may still be related to the Istio `port naming
conventions </faq/traffic-management/#naming-port-convention>`_.
Starting with `Istio
1.3 </news/releases/1.3.x/announcing-1.3/#intelligent-protocol-detection-experimental>`_
the protocol for **outbound** traffic is automatically detected.
