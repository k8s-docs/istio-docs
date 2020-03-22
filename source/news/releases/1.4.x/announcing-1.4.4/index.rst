announcing-1.4.4
==========================

This release includes bug fixes to improve robustness and user
experience as well as a fix for the security vulnerability described in
`our February 11th, 2020 news
post </news/security/istio-security-2020-001>`_. This release note
describes what’s different between Istio 1.4.3 and Istio 1.4.4.

{{< relnote >}}

Security update
---------------

-  **ISTIO-SECURITY-2020-001** An improper input validation has been
   discovered in ``AuthenticationPolicy``.

`CVE-2020-8595 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8595>`_:
A bug in Istio’s `Authentication
Policy </docs/reference/config/security/istio.authentication.v1alpha1/#Policy>`_
exact path matching logic allows unauthorized access to resources
without a valid JWT token.

Bug fixes
---------

-  **Fixed** Debian packaging of ``iptables`` scripts (`Issue
   19615 <https://github.com/istio/istio/issues/19615>`_).
-  **Fixed** an issue where Pilot generated a wrong Envoy configuration
   when the same port was used more than once (`Issue
   19935 <https://github.com/istio/istio/issues/19935>`_).
-  **Fixed** an issue where running multiple instances of Pilot could
   lead to a crash (`Issue
   20047 <https://github.com/istio/istio/issues/20047>`_).
-  **Fixed** a potential flood of configuration pushes from Pilot to
   Envoy when scaling the deployment to zero (`Issue
   17957 <https://github.com/istio/istio/issues/17957>`_).
-  **Fixed** an issue where Mixer could not fetch the correct
   information from the request/response when pod contains a dot in its
   name (`Issue 20028 <https://github.com/istio/istio/issues/20028>`_).
-  **Fixed** an issue where Pilot sometimes would not send a correct pod
   configuration to Envoy (`Issue
   19025 <https://github.com/istio/istio/issues/19025>`_).
-  **Fixed** an issue where sidecar injector with SDS enabled was
   overwriting pod ``securityContext`` section, instead of just patching
   it (`Issue 20409 <https://github.com/istio/istio/issues/20409>`_).

Improvements
------------

-  **Improved** Better compatibility with Google CA. (Issues
   `20530 <https://github.com/istio/istio/issues/20530>`_,
   `20560 <https://github.com/istio/istio/issues/20560>`_).
-  **Improved** Added analyzer error message when Policies using JWT are
   not configured properly (Issues
   `20884 <https://github.com/istio/istio/issues/20884>`_,
   `20767 <https://github.com/istio/istio/issues/20767>`_).
