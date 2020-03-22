announcing-1.2.7
===================

We’re pleased to announce the availability of Istio 1.2.7. Please see
below for what’s changed.

{{< relnote >}}

Security update
---------------

This release contains fixes for the security vulnerability described in
`our October 8th, 2019 news
post </news/security/istio-security-2019-005>`_. Specifically:

**ISTIO-SECURITY-2019-005**: A DoS vulnerability has been discovered by
the Envoy community. \*
`CVE-2019-15226 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-15226>`_:
After investigation, the Istio team has found that this issue could be
leveraged for a DoS attack in Istio if an attacker uses a high quantity
of very small headers.

Bug fix
-------

-  Fix a bug where ``nodeagent`` was failing to start when using citadel
   (`Issue 15876 <https://github.com/istio/istio/issues/17108>`_)
