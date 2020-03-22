announcing-1.4.2
==========================

This release contains fixes for the security vulnerability described in
`our December 10th, 2019 news
post </news/security/istio-security-2019-007>`_. This release note
describes what’s different between Istio 1.4.1 and Istio 1.4.2.

{{< relnote >}}

Security update
---------------

-  **ISTIO-SECURITY-2019-007** A heap overflow and improper input
   validation have been discovered in Envoy.

`CVE-2019-18801 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-18801>`_:
Fix a vulnerability affecting Envoy’s processing of large HTTP/2 request
headers. A successful exploitation of this vulnerability could lead to a
denial of service, escalation of privileges, or information disclosure.
`CVE-2019-18802 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-18802>`_:
Fix a vulnerability resulting from whitespace after HTTP/1 header values
which could allow an attacker to bypass Istio’s policy checks,
potentially resulting in information disclosure or escalation of
privileges.
`CVE-2019-18838 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-18838>`_:
Fix a vulnerability resulting from malformed HTTP request missing the
“Host” header. An encoder filter that invokes Envoy’s route manager APIs
that access request’s “Host” header will cause a NULL pointer to be
dereferenced and result in abnormal termination of the Envoy process.
