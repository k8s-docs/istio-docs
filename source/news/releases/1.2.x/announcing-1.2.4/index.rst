announcing-1.2.4
===================

We’re pleased to announce the availability of Istio 1.2.4. Please see
below for what’s changed.

{{< relnote >}}

Security update
---------------

This release contains fixes for the security vulnerabilities described
in
`ISTIO-SECURITY-2019-003 </news/security/istio-security-2019-003/>`_]
`ISTIO-SECURITY-2019-004 </news/security/istio-security-2019-004/>`_.
Specifically:

**ISTIO-SECURITY-2019-003**: An Envoy user reported publicly an issue
(c.f. `Envoy Issue
7728 <https://github.com/envoyproxy/envoy/issues/7728>`_) about regular
expressions matching that crashes Envoy with very large URIs. \*
`CVE-2019-14993 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-14993>`_:
After investigation, the Istio team has found that this issue could be
leveraged for a DoS attack in Istio, if users are employing regular
expressions in some of the Istio APIs: ``JWT``, ``VirtualService``,
``HTTPAPISpecBinding``, ``QuotaSpecBinding``.

**ISTIO-SECURITY-2019-004**: Envoy, and subsequently Istio are
vulnerable to a series of trivial HTTP/2-based DoS attacks: \*
`CVE-2019-9512 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9512>`_:
HTTP/2 flood using ``PING`` frames and queuing of response ``PING`` ACK
frames that results in unbounded memory growth (which can lead to out of
memory conditions). \*
`CVE-2019-9513 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9513>`_:
HTTP/2 flood using PRIORITY frames that results in excessive CPU usage
and starvation of other clients. \*
`CVE-2019-9514 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9514>`_:
HTTP/2 flood using ``HEADERS`` frames with invalid HTTP headers and
queuing of response ``RST_STREAM`` frames that results in unbounded
memory growth (which can lead to out of memory conditions). \*
`CVE-2019-9515 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9515>`_:
HTTP/2 flood using ``SETTINGS`` frames and queuing of ``SETTINGS`` ACK
frames that results in unbounded memory growth (which can lead to out of
memory conditions). \*
`CVE-2019-9518 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9518>`_:
HTTP/2 flood using frames with an empty payload that results in
excessive CPU usage and starvation of other clients.

Nothing else is included in this release except for the above security
fixes.
