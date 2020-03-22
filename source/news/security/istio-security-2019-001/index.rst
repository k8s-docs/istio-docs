istio-security-2019-001
===========================

{{< security_bulletin >}}

During review of the `Istio
1.1.7 </news/releases/1.1.x/announcing-1.1.7>`_ release notes, we
realized that `issue
13868 <https://github.com/istio/istio/issues/13868>`_, which is fixed
in the release, actually represents a security vulnerability.

Initially we thought the bug was impacting the `TCP
Authorization </about/feature-stages/#security-and-policy-enforcement>`_
feature advertised as alpha stability, which would not have required
invoking this security advisory process, but we later realized that the
`Deny
Checker </docs/reference/config/policy-and-telemetry/adapters/denier/>`_
and `List
Checker </docs/reference/config/policy-and-telemetry/adapters/list/>`_
feature were affected and those are considered stable features. We are
revisiting our processes to flag vulnerabilities that are initially
reported as bugs instead of through the `private disclosure
process </about/security-vulnerabilities/>`_.

We tracked the bug to a code change introduced in Istio 1.1 and
affecting all releases up to 1.1.6.

Impact and detection
--------------------

Since Istio 1.1, In the default Istio installation profile, policy
enforcement is disabled by default.

You can check the status of policy enforcement for your mesh with the
following command:

.. code:: sh

      $ kubectl -n istio-system get cm istio -o
jsonpath=“{@.data.mesh}” \| grep disablePolicyChecks
disablePolicyChecks: true

You are not impacted by this vulnerability if ``disablePolicyChecks`` is
set to true.

You are impacted by the vulnerability issue if the following conditions
are all true:

-  You are running one of the affected Istio releases.
-  ``disablePolicyChecks`` is set to false (follow the steps mentioned
   above to check)
-  Your workload is NOT using HTTP, HTTP/2, or gRPC protocols
-  A mixer adapter (e.g., Deny Checker, List Checker) is used to provide
   authorization for your backend TCP service.

Mitigation
----------

-  Users of Istio 1.0.x are not affected.
-  For Istio 1.1.x deployments: update to `Istio
   1.1.7 </news/releases/1.1.x/announcing-1.1.7>`_ or later.

Credit
------

The Istio team would like to thank ``Haim Helman`` for the original bug
report.

{{< boilerplate “security-vulnerability” >}}
