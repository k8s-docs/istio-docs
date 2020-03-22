announcing-1.2.8
===================

We’re pleased to announce the availability of Istio 1.2.8. Please see
below for what’s changed.

{{< relnote >}}

Bug fixes
---------

-  Fix a bug introduced by `our October 8th security
   release </news/security/istio-security-2019-005>`_ which incorrectly
   calculated HTTP header and body sizes (`Issue
   17735 <https://github.com/istio/istio/issues/17735>`_).

-  Fix a minor bug where endpoints still remained in /clusters while
   scaling a deployment to 0 replica (`Issue
   14336 <https://github.com/istio/istio/issues/14336>`_).

-  Fix Helm upgrade process to correctly update mesh policy for mutual
   TLS (`Issue 16170 <https://github.com/istio/istio/issues/16170>`_).

-  Fix inconsistencies in the destination service label for TCP
   connection opened/closed metrics (`Issue
   17234 <https://github.com/istio/istio/issues/17234>`_).

-  Fix the Istio secret cleanup mechanism (`Issue
   17122 <https://github.com/istio/istio/issues/17122>`_).

-  Fix the Mixer Stackdriver adapter encoding process to handle invalid
   UTF-8 (`Issue
   16966 <https://github.com/istio/istio/issues/16966>`_).

Features
--------

-  Add ``pilot`` support for the new failure domain labels: ``zone`` and
   ``region``.
