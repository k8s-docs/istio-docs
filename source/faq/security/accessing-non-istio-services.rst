accessing-non-istio-services
================================

Istio detects if the destination workload has an Envoy proxy and drops
mutual TLS if it doesn’t. Set an explicit destination rule to disable
mutual TLS. For example:

.. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
networking.istio.io/v1alpha3 kind: DestinationRule metadata: name:
“api-server” spec: host: “kubernetes.default.svc.cluster.local”
trafficPolicy: tls: mode: DISABLE EOF
