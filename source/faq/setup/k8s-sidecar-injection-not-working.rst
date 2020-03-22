k8s-sidecar-injection-not-working
=======================================

Ensure that your cluster has met the
`prerequisites </docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection>`_
for the automatic sidecar injection. If your microservice is deployed in
``kube-system``, ``kube-public`` or ``istio-system`` namespaces, they
are exempted from automatic sidecar injection. Please use a different
namespace instead.
