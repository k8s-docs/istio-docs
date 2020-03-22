istio-security-2019-007
==========================

{{< security_bulletin >}}

Envoy, and subsequently Istio are vulnerable to two newly discovered
vulnerabilities:

-  `CVE-2019-18801 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-18801>`_:
   This vulnerability affects Envoy’s HTTP/1 codec in its way it
   processes downstream’s requests with large HTTP/2 headers. A
   successful exploitation of this vulnerability could lead to a denial
   of Service, escalation of privileges, or information disclosure.

-  `CVE-2019-18802 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-18802>`_:
   HTTP/1 codec incorrectly fails to trim whitespace after header
   values. This could allow an attacker to bypass Istio’s policy either
   for information disclosure or escalation of privileges.

-  `CVE-2019-18838 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-18838>`_:
   Upon receipt of a malformed HTTP request without the “Host” header,
   an encoder filter invoking Envoy’s route manager APIs that access
   request’s “Host” header will cause a NULL pointer to be dereferenced
   and result in abnormal termination of the Envoy process.

Impact and detection
--------------------

Both Istio gateways and sidecars are vulnerable to this issue. If you
are running one of the affected releases where downstream’s requests are
HTTP/2 while upstream’s are HTTP/1, then your cluster is vulnerable. We
expect this to be true of most clusters.

Mitigation
----------

-  For Istio 1.2.x deployments: update to `Istio
   1.2.10 </news/releases/1.2.x/announcing-1.2.10>`_ or later.
-  For Istio 1.3.x deployments: update to `Istio
   1.3.6 </news/releases/1.3.x/announcing-1.3.6>`_ or later.
-  For Istio 1.4.x deployments: update to `Istio
   1.4.2 </news/releases/1.4.x/announcing-1.4.2>`_ or later.

{{< boilerplate “security-vulnerability” >}}
