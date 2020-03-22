initial-zipkin-header
==================================

The Istio sidecar proxy (Envoy) generates the initial
`headers <https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_conn_man/headers#x-request-id>`_,
if they are not provided by the request.
