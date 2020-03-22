disabling-tracing
==================================

If you already have installed Istio with tracing enabled, you can
disable it as follows:

{{< text plain >}} # Fill with the namespace of your istio mesh.Ex:
istio-system
TRACING_POD=\ ``kubectl get po -n <istio namespace> | grep istio-tracing | awk '{print $1}'``
$ kubectl delete pod $TRACING_POD -n $ kubectl delete services tracing
zipkin -n # Remove reference of zipkin url from mixer deployment $
kubectl -n istio-system edit deployment istio-telemetry # Now, manually
remove instances of trace_zipkin_url from the file and save it.

Then follow the steps of the `cleanup section of the Distributed Tracing
task </docs/tasks/observability/distributed-tracing/zipkin/#cleanup>`_.

If you don’t want tracing functionality at all, then `disable
tracing </docs/tasks/observability/distributed-tracing/zipkin/#before-you-begin>`_
when installing Istio.
