announcing-1.1-eol-final
=============================

As `previously announced </news/support/announcing-1.1-eol/>`_, support
for Istio 1.1 has now officially ended.

Since we learned of the security vulnerability `behind our October 8th
security release </news/security/istio-security-2019-005>`_ while still
barely within the 1.1 support period, we decided to extend the 1.1
support period beyond the original announcement and release
`1.1.16 </news/releases/1.1.x/announcing-1.1.16>`_. Then we discovered
a `bug in HTTP header size
calculation <https://github.com/istio/istio/issues/17735>`_ was
introduced by the security release, so we decided to release a fix in
one last `1.1.17 </news/releases/1.1.x/announcing-1.1.17>`_ release
before closing out the 1.1 series for good.

At this point we will no longer back-port fixes for security issues and
critical bugs to 1.1, so we heartily encourage you to upgrade to the
latest version of Istio ({{}}) if you haven’t already.
