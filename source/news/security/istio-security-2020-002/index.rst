istio-security-2020-002
=============================
{{< security_bulletin >}}

Istio 1.3 to 1.3.6 contain a vulnerability affecting Mixer policy
checks.

Note: We regret that the vulnerability was silently fixed in Istio 1.4.0
and Istio 1.3.7. An `issue was
raised <https://github.com/istio/istio/issues/12063>`_ and
`fixed <https://github.com/istio/istio/pull/17692>`_ in Istio 1.4.0 as
a non-security issue. We reclassified the issue as a vulnerability in
Dec 2019.

-  `CVE-2020-8843 <https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-8843>`_:
   Under certain circumstances it is possible to bypass a specifically
   configured Mixer policy. Istio-proxy accepts ``x-istio-attributes``
   header at ingress that can be used to affect policy decisions when
   Mixer policy selectively applies to source equal to ingress. To be
   vulnerable, Istio must have Mixer Policy enabled and used in the
   specified way. This feature is disabled by default in Istio 1.3 and
   1.4.

Mitigation
----------

-  For Istio 1.3.x deployments: update to `Istio
   1.3.7 </news/releases/1.3.x/announcing-1.3.7>`_ or later.

Credit
------

The Istio team would like to thank Krishnan Anantheswaran and Eric Zhang
of `Splunk <https://www.splunk.com/>`_ for the private bug report.

{{< boilerplate “security-vulnerability” >}}
