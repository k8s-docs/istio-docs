Attribute vocabulary
============================

Attributes are a central concept used throughout Istio. You can find a
description of what attributes are and what they are used for
`here </docs/reference/config/policy-and-telemetry/mixer-overview/#attributes>`_.

A given Istio deployment has a fixed vocabulary of attributes that it
understands. The specific vocabulary is determined by the set of
attribute producers being used in the deployment. The primary attribute
producer in Istio is Envoy, although Mixer and services can also
introduce attributes.

The table below shows the set of canonical attributes and their
respective types. Most Istio deployments will have agents (Envoy or
Mixer adapters) that produce these attributes.

+--------+--------+-------------------+-------------------------------+
| Name   | Type   | Description       | Kubernetes Example            |
+========+========+===================+===============================+
| ``sour | string | Platform-specific | ``kubernetes://redis-master-2 |
| ce.uid |        | unique identifier | 353460263-1ecey.my-namespace` |
| ``     |        | for the source    | `                             |
|        |        | workload          |                               |
|        |        | instance.         |                               |
+--------+--------+-------------------+-------------------------------+
| ``sour | ip_add | Source workload   | ``10.0.0.117``                |
| ce.ip` | ress   | instance IP       |                               |
| `      |        | address.          |                               |
+--------+--------+-------------------+-------------------------------+
| ``sour | map[st | A map of          | version => v1                 |
| ce.lab | ring,  | key-value pairs   |                               |
| els``  | string | attached to the   |                               |
|        | ]      | source instance.  |                               |
+--------+--------+-------------------+-------------------------------+
| ``sour | string | Source workload   | ``redis-master-2353460263-1ec |
| ce.nam |        | instance name.    | ey``                          |
| e``    |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``sour | string | Source workload   | ``my-namespace``              |
| ce.nam |        | instance          |                               |
| espace |        | namespace.        |                               |
| ``     |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``sour | string | Authority under   | ``service-account-foo``       |
| ce.pri |        | which the source  |                               |
| ncipal |        | workload instance |                               |
| ``     |        | is running.       |                               |
+--------+--------+-------------------+-------------------------------+
| ``sour | string | Reference to the  | ``kubernetes://apis/extension |
| ce.own |        | workload          | s/v1beta1/namespaces/istio-sy |
| er``   |        | controlling the   | stem/deployments/istio-policy |
|        |        | source workload   | ``                            |
|        |        | instance.         |                               |
+--------+--------+-------------------+-------------------------------+
| ``sour | string | Unique identifier | ``istio://istio-system/worklo |
| ce.wor |        | of the source     | ads/istio-policy``            |
| kload. |        | workload.         |                               |
| uid``  |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``sour | string | Source workload   | ``istio-policy``              |
| ce.wor |        | name.             |                               |
| kload. |        |                   |                               |
| name`` |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``sour | string | Source workload   | ``istio-system``              |
| ce.wor |        | namespace.        |                               |
| kload. |        |                   |                               |
| namesp |        |                   |                               |
| ace``  |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Platform-specific | ``kubernetes://my-svc-234443- |
| inatio |        | unique identifier | 5sffe.my-namespace``          |
| n.uid` |        | for the server    |                               |
| `      |        | instance.         |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | ip_add | Server IP         | ``10.0.0.104``                |
| inatio | ress   | address.          |                               |
| n.ip`` |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | int64  | The recipient     | ``8080``                      |
| inatio |        | port on the       |                               |
| n.port |        | server IP         |                               |
| ``     |        | address.          |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | map[st | A map of          | version => v2                 |
| inatio | ring,  | key-value pairs   |                               |
| n.labe | string | attached to the   |                               |
| ls``   | ]      | server instance.  |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Destination       | ``istio-telemetry-2359333``   |
| inatio |        | workload instance |                               |
| n.name |        | name.             |                               |
| ``     |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Destination       | ``istio-system``              |
| inatio |        | workload instance |                               |
| n.name |        | namespace.        |                               |
| space` |        |                   |                               |
| `      |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Authority under   | ``service-account``           |
| inatio |        | which the         |                               |
| n.prin |        | destination       |                               |
| cipal` |        | workload instance |                               |
| `      |        | is running.       |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Reference to the  | ``kubernetes://apis/extension |
| inatio |        | workload          | s/v1beta1/namespaces/istio-sy |
| n.owne |        | controlling the   | stem/deployments/istio-teleme |
| r``    |        | destination       | try``                         |
|        |        | workload          |                               |
|        |        | instance.         |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Unique identifier | ``istio://istio-system/worklo |
| inatio |        | of the            | ads/istio-telemetry``         |
| n.work |        | destination       |                               |
| load.u |        | workload.         |                               |
| id``   |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Destination       | ``istio-telemetry``           |
| inatio |        | workload name.    |                               |
| n.work |        |                   |                               |
| load.n |        |                   |                               |
| ame``  |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Destination       | ``istio-system``              |
| inatio |        | workload          |                               |
| n.work |        | namespace.        |                               |
| load.n |        |                   |                               |
| amespa |        |                   |                               |
| ce``   |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Name of the       | ``mixer``                     |
| inatio |        | destination       |                               |
| n.cont |        | workload          |                               |
| ainer. |        | instance’s        |                               |
| name`` |        | container.        |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Image of the      | ``gcr.io/istio-testing/mixer: |
| inatio |        | destination       | 0.8.0``                       |
| n.cont |        | workload          |                               |
| ainer. |        | instance’s        |                               |
| image` |        | container.        |                               |
| `      |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Destination host  | ``istio-telemetry.istio-syste |
| inatio |        | address.          | m.svc.cluster.local``         |
| n.serv |        |                   |                               |
| ice.ho |        |                   |                               |
| st``   |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Unique identifier | ``istio://istio-system/servic |
| inatio |        | of the            | es/istio-telemetry``          |
| n.serv |        | destination       |                               |
| ice.ui |        | service.          |                               |
| d``    |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Destination       | ``istio-telemetry``           |
| inatio |        | service name.     |                               |
| n.serv |        |                   |                               |
| ice.na |        |                   |                               |
| me``   |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``dest | string | Destination       | ``istio-system``              |
| inatio |        | service           |                               |
| n.serv |        | namespace.        |                               |
| ice.na |        |                   |                               |
| mespac |        |                   |                               |
| e``    |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``orig | ip_add | IP address of the | ``127.0.0.1``                 |
| in.ip` | ress   | proxy client,     |                               |
| `      |        | e.g. origin for   |                               |
|        |        | the ingress       |                               |
|        |        | proxies.          |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | map[st | HTTP request      |                               |
| est.he | ring,  | headers with      |                               |
| aders` | string | lowercase keys.   |                               |
| `      | ]      | For gRPC, its     |                               |
|        |        | metadata will be  |                               |
|        |        | here.             |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | An ID for the     |                               |
| est.id |        | request with      |                               |
| ``     |        | statistically low |                               |
|        |        | probability of    |                               |
|        |        | collision.        |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | The HTTP URL path |                               |
| est.pa |        | including query   |                               |
| th``   |        | string            |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | The path part of  |                               |
| est.ur |        | HTTP URL, with    |                               |
| l_path |        | query string      |                               |
| ``     |        | being stripped    |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | map[st | A map of query    |                               |
| est.qu | ring,  | parameters        |                               |
| ery_pa | string | extracted from    |                               |
| rams`` | ]      | the HTTP URL.     |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | HTTP/1.x host     | ``redis-master:3337``         |
| est.ho |        | header or HTTP/2  |                               |
| st``   |        | authority header. |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | The HTTP method.  |                               |
| est.me |        |                   |                               |
| thod`` |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | The request       |                               |
| est.re |        | reason used by    |                               |
| ason`` |        | auditing systems. |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | The HTTP referer  |                               |
| est.re |        | header.           |                               |
| ferer` |        |                   |                               |
| `      |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | URI Scheme of the |                               |
| est.sc |        | request           |                               |
| heme`` |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | int64  | Size of the       |                               |
| est.si |        | request in bytes. |                               |
| ze``   |        | For HTTP requests |                               |
|        |        | this is           |                               |
|        |        | equivalent to the |                               |
|        |        | Content-Length    |                               |
|        |        | header.           |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | int64  | Total size of     |                               |
| est.to |        | HTTP request in   |                               |
| tal_si |        | bytes, including  |                               |
| ze``   |        | request headers,  |                               |
|        |        | body and          |                               |
|        |        | trailers.         |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | timest | The timestamp     |                               |
| est.ti | amp    | when the          |                               |
| me``   |        | destination       |                               |
|        |        | receives the      |                               |
|        |        | request. This     |                               |
|        |        | should be         |                               |
|        |        | equivalent to     |                               |
|        |        | Firebase “now”.   |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | The HTTP          |                               |
| est.us |        | User-Agent        |                               |
| eragen |        | header.           |                               |
| t``    |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``resp | map[st | HTTP response     |                               |
| onse.h | ring,  | headers with      |                               |
| eaders | string | lowercase keys.   |                               |
| ``     | ]      |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``resp | int64  | Size of the       |                               |
| onse.s |        | response body in  |                               |
| ize``  |        | bytes             |                               |
+--------+--------+-------------------+-------------------------------+
| ``resp | int64  | Total size of     |                               |
| onse.t |        | HTTP response in  |                               |
| otal_s |        | bytes, including  |                               |
| ize``  |        | response headers  |                               |
|        |        | and body.         |                               |
+--------+--------+-------------------+-------------------------------+
| ``resp | timest | The timestamp     |                               |
| onse.t | amp    | when the          |                               |
| ime``  |        | destination       |                               |
|        |        | produced the      |                               |
|        |        | response.         |                               |
+--------+--------+-------------------+-------------------------------+
| ``resp | durati | The amount of     |                               |
| onse.d | on     | time the response |                               |
| uratio |        | took to generate. |                               |
| n``    |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``resp | int64  | The response’s    |                               |
| onse.c |        | HTTP status code. |                               |
| ode``  |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``resp | string | The response’s    |                               |
| onse.g |        | gRPC status.      |                               |
| rpc_st |        |                   |                               |
| atus`` |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``resp | string | The response’s    |                               |
| onse.g |        | gRPC status       |                               |
| rpc_me |        | message.          |                               |
| ssage` |        |                   |                               |
| `      |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``conn | string | An ID for a TCP   |                               |
| ection |        | connection with   |                               |
| .id``  |        | statistically low |                               |
|        |        | probability of    |                               |
|        |        | collision.        |                               |
+--------+--------+-------------------+-------------------------------+
| ``conn | string | Status of a TCP   |                               |
| ection |        | connection, its   |                               |
| .event |        | value is one of   |                               |
| ``     |        | “open”,           |                               |
|        |        | “continue” and    |                               |
|        |        | “close”.          |                               |
+--------+--------+-------------------+-------------------------------+
| ``conn | int64  | Number of bytes   |                               |
| ection |        | received by a     |                               |
| .recei |        | destination       |                               |
| ved.by |        | service on a      |                               |
| tes``  |        | connection since  |                               |
|        |        | the last Report() |                               |
|        |        | for a connection. |                               |
+--------+--------+-------------------+-------------------------------+
| ``conn | int64  | Total number of   |                               |
| ection |        | bytes received by |                               |
| .recei |        | a destination     |                               |
| ved.by |        | service during    |                               |
| tes_to |        | the lifetime of a |                               |
| tal``  |        | connection.       |                               |
+--------+--------+-------------------+-------------------------------+
| ``conn | int64  | Number of bytes   |                               |
| ection |        | sent by a         |                               |
| .sent. |        | destination       |                               |
| bytes` |        | service on a      |                               |
| `      |        | connection since  |                               |
|        |        | the last Report() |                               |
|        |        | for a connection. |                               |
+--------+--------+-------------------+-------------------------------+
| ``conn | int64  | Total number of   |                               |
| ection |        | bytes sent by a   |                               |
| .sent. |        | destination       |                               |
| bytes_ |        | service during    |                               |
| total` |        | the lifetime of a |                               |
| `      |        | connection.       |                               |
+--------+--------+-------------------+-------------------------------+
| ``conn | durati | The total amount  |                               |
| ection | on     | of time a         |                               |
| .durat |        | connection has    |                               |
| ion``  |        | been open.        |                               |
+--------+--------+-------------------+-------------------------------+
| ``conn | boolea | Indicates whether |                               |
| ection | n      | a request is      |                               |
| .mtls` |        | received over a   |                               |
| `      |        | mutual TLS        |                               |
|        |        | enabled           |                               |
|        |        | downstream        |                               |
|        |        | connection.       |                               |
+--------+--------+-------------------+-------------------------------+
| ``conn | string | The requested     |                               |
| ection |        | server name (SNI) |                               |
| .reque |        | of the connection |                               |
| sted_s |        |                   |                               |
| erver_ |        |                   |                               |
| name`` |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``cont | string | Protocol of the   | ``tcp``                       |
| ext.pr |        | request or        |                               |
| otocol |        | connection being  |                               |
| ``     |        | proxied.          |                               |
+--------+--------+-------------------+-------------------------------+
| ``cont | timest | The timestamp of  |                               |
| ext.ti | amp    | Mixer operation.  |                               |
| me``   |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``cont | string | Contextualizes    | ``inbound``                   |
| ext.re |        | the reported      |                               |
| porter |        | attribute set.    |                               |
| .kind` |        | Set to            |                               |
| `      |        | ``inbound`` for   |                               |
|        |        | the server-side   |                               |
|        |        | calls from        |                               |
|        |        | sidecars and      |                               |
|        |        | ``outbound`` for  |                               |
|        |        | the client-side   |                               |
|        |        | calls from        |                               |
|        |        | sidecars and      |                               |
|        |        | gateways          |                               |
+--------+--------+-------------------+-------------------------------+
| ``cont | string | Platform-specific | ``kubernetes://my-svc-234443- |
| ext.re |        | identifier of the | 5sffe.my-namespace``          |
| porter |        | attribute         |                               |
| .uid`` |        | reporter.         |                               |
+--------+--------+-------------------+-------------------------------+
| ``cont | string | Additional        | ``UH``                        |
| ext.pr |        | details about the |                               |
| oxy_er |        | response or       |                               |
| ror_co |        | connection from   |                               |
| de``   |        | proxy. In case of |                               |
|        |        | Envoy, see        |                               |
|        |        | ``%RESPONSE_FLAGS |                               |
|        |        | %``               |                               |
|        |        | in `Envoy Access  |                               |
|        |        | Log <https://www. |                               |
|        |        | envoyproxy.io/doc |                               |
|        |        | s/envoy/latest/co |                               |
|        |        | nfiguration/obser |                               |
|        |        | vability/access_l |                               |
|        |        | og#configuration> |                               |
|        |        | `_               |                               |
|        |        | for more detail   |                               |
+--------+--------+-------------------+-------------------------------+
| ``api. | string | The public        | ``my-svc.com``                |
| servic |        | service name.     |                               |
| e``    |        | This is different |                               |
|        |        | than the in-mesh  |                               |
|        |        | service identity  |                               |
|        |        | and reflects the  |                               |
|        |        | name of the       |                               |
|        |        | service exposed   |                               |
|        |        | to the client.    |                               |
+--------+--------+-------------------+-------------------------------+
| ``api. | string | The API version.  | ``v1alpha1``                  |
| versio |        |                   |                               |
| n``    |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``api. | string | Unique string     | ``getPetsById``               |
| operat |        | used to identify  |                               |
| ion``  |        | the operation.    |                               |
|        |        | The id is unique  |                               |
|        |        | among all         |                               |
|        |        | operations        |                               |
|        |        | described in a    |                               |
|        |        | specific          |                               |
|        |        | <service,         |                               |
|        |        | version>.         |                               |
+--------+--------+-------------------+-------------------------------+
| ``api. | string | The protocol type | ``http``, ``https``, or       |
| protoc |        | of the API call.  | ``grpc``                      |
| ol``   |        | Mainly for        |                               |
|        |        | monitoring/analyt |                               |
|        |        | ics.              |                               |
|        |        | Note that this is |                               |
|        |        | the frontend      |                               |
|        |        | protocol exposed  |                               |
|        |        | to the client,    |                               |
|        |        | not the protocol  |                               |
|        |        | implemented by    |                               |
|        |        | the backend       |                               |
|        |        | service.          |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | The authenticated | ``issuer@foo.com/sub@foo.com` |
| est.au |        | principal of the  | `                             |
| th.pri |        | request. This is  |                               |
| ncipal |        | a string of the   |                               |
| ``     |        | issuer (``iss``)  |                               |
|        |        | and subject       |                               |
|        |        | (``sub``) claims  |                               |
|        |        | within a JWT      |                               |
|        |        | concatenated with |                               |
|        |        | “/” with a        |                               |
|        |        | percent-encoded   |                               |
|        |        | subject value.    |                               |
|        |        | This attribute    |                               |
|        |        | may come from the |                               |
|        |        | peer or the       |                               |
|        |        | origin in the     |                               |
|        |        | Istio             |                               |
|        |        | authentication    |                               |
|        |        | policy, depending |                               |
|        |        | on the binding    |                               |
|        |        | rule defined in   |                               |
|        |        | the Istio         |                               |
|        |        | authentication    |                               |
|        |        | policy.           |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | The intended      | ``aud1``                      |
| est.au |        | audience(s) for   |                               |
| th.aud |        | this              |                               |
| iences |        | authentication    |                               |
| ``     |        | information. This |                               |
|        |        | should reflect    |                               |
|        |        | the audience      |                               |
|        |        | (``aud``) claim   |                               |
|        |        | within a JWT.     |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | The authorized    | 123456789012.my-svc.com       |
| est.au |        | presenter of the  |                               |
| th.pre |        | credential. This  |                               |
| senter |        | value should      |                               |
| ``     |        | reflect the       |                               |
|        |        | optional          |                               |
|        |        | Authorized        |                               |
|        |        | Presenter         |                               |
|        |        | (``azp``) claim   |                               |
|        |        | within a JWT or   |                               |
|        |        | the OAuth2 client |                               |
|        |        | id.               |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | map[st | all raw string    | ``iss``: ``issuer@foo.com``,  |
| est.au | ring,  | claims from the   | ``sub``: ``sub@foo.com``,     |
| th.cla | string | ``origin`` JWT    | ``aud``: ``aud1``             |
| ims``  | ]      |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``requ | string | The API key used  | abcde12345                    |
| est.ap |        | for the request.  |                               |
| i_key` |        |                   |                               |
| `      |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``chec | int64  | The error         | 5                             |
| k.erro |        | `code <https://gi |                               |
| r_code |        | thub.com/google/p |                               |
| ``     |        | rotobuf/blob/mast |                               |
|        |        | er/src/google/pro |                               |
|        |        | tobuf/stubs/statu |                               |
|        |        | s.h>`_           |                               |
|        |        | for Mixer Check   |                               |
|        |        | call.             |                               |
+--------+--------+-------------------+-------------------------------+
| ``chec | string | The error message | Could not find the resource   |
| k.erro |        | for Mixer Check   |                               |
| r_mess |        | call.             |                               |
| age``  |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``chec | boolea | Indicates whether |                               |
| k.cach | n      | Mixer check call  |                               |
| e_hit` |        | hits local cache. |                               |
| `      |        |                   |                               |
+--------+--------+-------------------+-------------------------------+
| ``quot | boolea | Indicates whether |                               |
| a.cach | n      | Mixer quota call  |                               |
| e_hit` |        | hits local cache. |                               |
| `      |        |                   |                               |
+--------+--------+-------------------+-------------------------------+

Timestamp and duration attributes format
----------------------------------------

Timestamp attributes are represented in the RFC 3339 format. When
operating with timestamp attributes, you can use the ``timestamp``
function defined in
`CEXL </docs/reference/config/policy-and-telemetry/expression-language/>`_
to convert a textual timestamp in RFC 3339 format into the ``TIMESTAMP``
type, for example:
``request.time | timestamp("2018-01-01T22:08:41+00:00")``,
``response.time > timestamp("2020-02-29T00:00:00-08:00")``.

Duration attributes represent an amount of time, expressed as a series
of decimal numbers with an optional fractional part denoted with a
period, and a unit value. The possible unit values are ``ns`` for
nanoseconds, ``us`` (or ``µs``) for microseconds, ``ms`` for
milliseconds, ``s`` for seconds, ``m`` for minutes, ``h`` for hours. For
example:

-  ``1ms`` represents 1 millisecond
-  ``2.3s`` represents 2.3 seconds
-  ``4m`` represents 4 minutes
-  ``5h10m`` represents 5 hours and 10 minutes
