announcing-1.3.8
==========================

This release contains a fix for the security vulnerability described in
`our February 11th, 2020 news
post </news/security/istio-security-2020-001>`_. This release note
describes what’s different between Istio 1.3.7 and Istio 1.3.8.

{{< relnote >}}

Security update
---------------

-  **ISTIO-SECURITY-2020-001** Improper input validation have been
   discovered in ``AuthenticationPolicy``.

`CVE-2020-8595 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8595>`_:
A bug in Istio’s `Authentication
Policy </docs/reference/config/security/istio.authentication.v1alpha1/#Policy>`_
exact path matching logic allows unauthorized access to resources
without a valid JWT token.
