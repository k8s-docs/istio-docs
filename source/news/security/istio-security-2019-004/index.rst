istio-security-2019-004
=========================

{{< security_bulletin >}}

Envoy, and subsequently Istio are vulnerable to a series of trivial
HTTP/2-based DoS attacks:

-  HTTP/2 flood using PING frames and queuing of response PING ACK
   frames that results in unbounded memory growth (which can lead to out
   of memory conditions).
-  HTTP/2 flood using PRIORITY frames that results in excessive CPU
   usage and starvation of other clients.
-  HTTP/2 flood using HEADERS frames with invalid HTTP headers and
   queuing of response ``RST_STREAM`` frames that results in unbounded
   memory growth (which can lead to out of memory conditions).
-  HTTP/2 flood using SETTINGS frames and queuing of SETTINGS ACK frames
   that results in unbounded memory growth (which can lead to out of
   memory conditions).
-  HTTP/2 flood using frames with an empty payload that results in
   excessive CPU usage and starvation of other clients.

Those vulnerabilities were reported externally and affect multiple proxy
implementations. See `this security
bulletin <https://github.com/Netflix/security-bulletins/blob/master/advisories/third-party/2019-002.md>`_
for more information.

Impact and detection
--------------------

If Istio terminates externally originated HTTP then it is vulnerable. If
Istio is instead fronted by an intermediary that terminates HTTP (e.g.,
a HTTP load balancer), then that intermediary would protect Istio,
assuming the intermediary is not itself vulnerable to the same HTTP/2
exploits.

Mitigation
----------

-  For Istio 1.1.x deployments: update to a `Istio
   1.1.13 </news/releases/1.1.x/announcing-1.1.13>`_ or later.
-  For Istio 1.2.x deployments: update to a `Istio
   1.2.4 </news/releases/1.2.x/announcing-1.2.4>`_ or later.

{{< boilerplate “security-vulnerability” >}}
