Monitoring and Policies for TLS Egress with Mixer (Deprecated)
========================================================================

The `Configure Egress Traffic using Wildcard
Hosts </docs/tasks/traffic-management/egress/wildcard-egress-hosts/>`_
example describes how to enable TLS egress traffic for a set of hosts in
a common domain, in that case ``*.wikipedia.org``. This example extends
that example to show how to configure SNI monitoring and apply policies
on TLS egress traffic.

{{< boilerplate before-you-begin-egress >}}

-  `Deploy Istio egress
   gateway </docs/tasks/traffic-management/egress/egress-gateway/#deploy-istio-egress-gateway>`_.

-  `Enable Envoy’s access
   logging </docs/tasks/observability/logs/access-log/#enable-envoy-s-access-logging>`_

-  Configure traffic to ``*.wikipedia.org`` by following `the
   steps </docs/tasks/traffic-management/egress/wildcard-egress-hosts/#wildcard-configuration-for-arbitrary-domains>`_
   in `Configure Egress Traffic using Wildcard
   Hosts </docs/tasks/traffic-management/egress/wildcard-egress-hosts/>`_
   example, **with mutual TLS enabled**.

   .. warning::

   Policy enforcement **must** be enabled in your
   cluster for this task. Follow the steps in `Enabling Policy
   Enforcement </docs/tasks/policy-enforcement/enabling-policy/>`_ to
   ensure that policy enforcement is enabled.

SNI monitoring and access policies
----------------------------------

Since you configured the egress traffic to flow through the egress
gateway, you can apply monitoring and access policy enforcement on the
egress traffic, **securely**. In this section you will define a log
entry and an access policy for the egress traffic to \_*.wikipedia.org_.

1. Create logging configuration:

   .. code:: sh

      $ kubectl apply -f
   @samples/sleep/telemetry/sni-logging.yaml@

2. Send HTTPS requests to https://en.wikipedia.org and
   https://de.wikipedia.org:

   .. code:: sh

      $ kubectl exec -it $SOURCE_POD -c sleep – sh -c
   ’curl -s https://en.wikipedia.org/wiki/Main_Page \| grep -o "

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   “; curl -s https://de.wikipedia.org/wiki/Wikipedia:Hauptseite \| grep
   -o”

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   "’

   .. raw:: html

      <title>

   Wikipedia, the free encyclopedia

   .. raw:: html

      </title>

   .. raw:: html

      <title>

   Wikipedia – Die freie Enzyklopädie

   .. raw:: html

      </title>



3. Check the mixer log. If Istio is deployed in the ``istio-system``
   namespace, the command to print the log is:

   .. code:: sh

      $ kubectl -n istio-system logs -l
   istio-mixer-type=telemetry -c mixer \| grep ‘egress-access’ {{< /text
   >}}

4. Define a policy that allows access to the hostnames matching
   ``*.wikipedia.org`` except for Wikipedia in English:

   .. code:: sh

      $ kubectl apply -f
   @samples/sleep/policy/sni-wikipedia.yaml@

5. Send an HTTPS request to the blacklisted `Wikipedia in
   English <https://en.wikipedia.org>`_:

   .. code:: sh

      $ kubectl exec -it $SOURCE_POD -c sleep – sh -c
   ‘curl -v https://en.wikipedia.org/wiki/Main_Page’ … curl: (35)
   Unknown SSL protocol error in connection to en.wikipedia.org:443
   command terminated with exit code 35

   Access to Wikipedia in English is blocked according to the policy you
   defined.

6. Send HTTPS requests to some other Wikipedia sites, for example
   https://es.wikipedia.org and https://de.wikipedia.org:

   .. code:: sh

      $ kubectl exec -it $SOURCE_POD -c sleep – sh -c
   ’curl -s https://es.wikipedia.org/wiki/Wikipedia:Portada \| grep -o "

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   “; curl -s https://de.wikipedia.org/wiki/Wikipedia:Hauptseite \| grep
   -o”

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   "’

   .. raw:: html

      <title>

   Wikipedia, la enciclopedia libre

   .. raw:: html

      </title>

   .. raw:: html

      <title>

   Wikipedia – Die freie Enzyklopädie

   .. raw:: html

      </title>



   Access to Wikipedia sites in other languages is allowed, as expected.

Cleanup of monitoring and policy enforcement
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sh

      $ kubectl delete -f
@samples/sleep/telemetry/sni-logging.yaml@ $ kubectl delete -f
@samples/sleep/policy/sni-wikipedia.yaml@

Monitor the SNI and the source identity, and enforce access policies based on them
----------------------------------------------------------------------------------

Since you enabled mutual TLS between the sidecar proxies and the egress
gateway, you can monitor the `service
identity </docs/ops/deployment/architecture/#citadel>`_ of the
applications that access external services, and enforce policies based
on the identities of the traffic source. In Istio on Kubernetes, the
identities are based on `Service
Accounts <https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/>`_.
In this subsection, you deploy two *sleep* containers, ``sleep-us`` and
``sleep-canada`` under two service accounts, ``sleep-us`` and
``sleep-canada``, respectively. Then you define a policy that allows
applications with the ``sleep-us`` identity to access the English and
the Spanish versions of Wikipedia, and services with ``sleep-canada``
identity to access the English and the French versions.

1. Deploy two *sleep* containers, ``sleep-us`` and ``sleep-canada``,
   with ``sleep-us`` and ``sleep-canada`` service accounts,
   respectively:

   .. code:: sh

      $ sed ‘s/: sleep/: sleep-us/g’
   @samples/sleep/sleep.yaml@ \| kubectl apply -f - $ sed ‘s/: sleep/:
   sleep-canada/g’ @samples/sleep/sleep.yaml@ \| kubectl apply -f -
   serviceaccount “sleep-us” created service “sleep-us” created
   deployment “sleep-us” created serviceaccount “sleep-canada” created
   service “sleep-canada” created deployment “sleep-canada” created

2. Create logging configuration:

   .. code:: sh

      $ kubectl apply -f
   @samples/sleep/telemetry/sni-logging.yaml@

3. Send HTTPS requests to Wikipedia sites in English, German, Spanish
   and French, from ``sleep-us``:

   .. code:: sh

      $ kubectl exec -it $(kubectl get pod -l
   app=sleep-us -o jsonpath=‘{.items[0].metadata.name}’) -c sleep-us –
   sh -c ’curl -s https://en.wikipedia.org/wiki/Main_Page \| grep -o "

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   “; curl -s https://de.wikipedia.org/wiki/Wikipedia:Hauptseite \| grep
   -o”

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   “; curl -s https://es.wikipedia.org/wiki/Wikipedia:Portada \| grep
   -o”

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   “; curl -s
   https://fr.wikipedia.org/wiki/Wikip%C3%A9dia:Accueil_principal \|
   grep -o”

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   "’

   .. raw:: html

      <title>

   Wikipedia, the free encyclopedia

   .. raw:: html

      </title>

   .. raw:: html

      <title>

   Wikipedia – Die freie Enzyklopädie

   .. raw:: html

      </title>

   .. raw:: html

      <title>

   Wikipedia, la enciclopedia libre

   .. raw:: html

      </title>

   .. raw:: html

      <title>

   Wikipédia, l’encyclopédie libre

   .. raw:: html

      </title>



4. Check the mixer log. If Istio is deployed in the ``istio-system``
   namespace, the command to print the log is:

   .. code:: sh

      $ kubectl -n istio-system logs -l
   istio-mixer-type=telemetry -c mixer \| grep ‘egress-access’
   {“level”:“info”,“time”:“2019-01-10T17:33:55.559093Z”,“instance”:“egress-access.instance.istio-system”,“connectionEvent”:“open”,“destinationApp”:"“,”requestedServerName“:”en.wikipedia.org“,”source“:”istio-egressgateway-with-sni-proxy“,”sourceNamespace“:”default“,”sourcePrincipal“:”cluster.local/ns/default/sa/sleep-us“,”sourceWorkload“:”istio-egressgateway-with-sni-proxy“}
   {”level“:”info“,”time“:”2019-01-10T17:33:56.166227Z“,”instance“:”egress-access.instance.istio-system“,”connectionEvent“:”open“,”destinationApp“:”“,”requestedServerName“:”de.wikipedia.org“,”source“:”istio-egressgateway-with-sni-proxy“,”sourceNamespace“:”default“,”sourcePrincipal“:”cluster.local/ns/default/sa/sleep-us“,”sourceWorkload“:”istio-egressgateway-with-sni-proxy“}
   {”level“:”info“,”time“:”2019-01-10T17:33:56.779842Z“,”instance“:”egress-access.instance.istio-system“,”connectionEvent“:”open“,”destinationApp“:”“,”requestedServerName“:”es.wikipedia.org“,”source“:”istio-egressgateway-with-sni-proxy“,”sourceNamespace“:”default“,”sourcePrincipal“:”cluster.local/ns/default/sa/sleep-us“,”sourceWorkload“:”istio-egressgateway-with-sni-proxy“}
   {”level“:”info“,”time“:”2019-01-10T17:33:57.413908Z“,”instance“:”egress-access.instance.istio-system“,”connectionEvent“:”open“,”destinationApp“:”“,”requestedServerName“:”fr.wikipedia.org“,”source“:”istio-egressgateway-with-sni-proxy“,”sourceNamespace“:”default“,”sourcePrincipal“:”cluster.local/ns/default/sa/sleep-us“,”sourceWorkload“:”istio-egressgateway-with-sni-proxy"}


   Note the ``requestedServerName`` attribute, and ``sourcePrincipal``,
   it must be ``cluster.local/ns/default/sa/sleep-us``.

5. Define a policy that will allow access to Wikipedia in English and
   Spanish for applications with the ``sleep-us`` service account and to
   Wikipedia in English and French for applications with the
   ``sleep-canada`` service account. Access to other Wikipedia sites
   will be blocked.

   .. code:: sh

      $ kubectl apply -f
   @samples/sleep/policy/sni-serviceaccount.yaml@

6. Resend HTTPS requests to Wikipedia sites in English, German, Spanish
   and French, from ``sleep-us``:

   .. code:: sh

      $ kubectl exec -it $(kubectl get pod -l
   app=sleep-us -o jsonpath=‘{.items[0].metadata.name}’) -c sleep-us –
   sh -c ’curl -s https://en.wikipedia.org/wiki/Main_Page \| grep -o "

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   “; curl -s https://de.wikipedia.org/wiki/Wikipedia:Hauptseite \| grep
   -o”

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   “; curl -s https://es.wikipedia.org/wiki/Wikipedia:Portada \| grep
   -o”

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   “; curl -s
   https://fr.wikipedia.org/wiki/Wikip%C3%A9dia:Accueil_principal \|
   grep -o”

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   ";:’

   .. raw:: html

      <title>

   Wikipedia, the free encyclopedia

   .. raw:: html

      </title>

   .. raw:: html

      <title>

   Wikipedia, la enciclopedia libre

   .. raw:: html

      </title>



   Note that only the allowed Wikipedia sites for ``sleep-us`` service
   account are allowed, namely Wikipedia in English and Spanish.

   .. note::

   It may take several minutes for the Mixer policy
   components to synchronize on the new policy. In case you want to
   quickly demonstrate the new policy without waiting until the
   synchronization is complete, delete the Mixer policy pods: {{< /tip
   >}}

   .. code:: sh

      $ kubectl delete pod -n istio-system -l
   istio-mixer-type=policy

7. Resend HTTPS requests to Wikipedia sites in English, German, Spanish
   and French, from ``sleep-canada``:

   .. code:: sh

      $ kubectl exec -it $(kubectl get pod -l
   app=sleep-canada -o jsonpath=‘{.items[0].metadata.name}’) -c
   sleep-canada – sh -c ’curl -s https://en.wikipedia.org/wiki/Main_Page
   \| grep -o "

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   “; curl -s https://de.wikipedia.org/wiki/Wikipedia:Hauptseite \| grep
   -o”

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   “; curl -s https://es.wikipedia.org/wiki/Wikipedia:Portada \| grep
   -o”

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   “; curl -s
   https://fr.wikipedia.org/wiki/Wikip%C3%A9dia:Accueil_principal \|
   grep -o”

   .. raw:: html

      <title>

   .\*

   .. raw:: html

      </title>

   ";:’

   .. raw:: html

      <title>

   Wikipedia, the free encyclopedia

   .. raw:: html

      </title>

   .. raw:: html

      <title>

   Wikipédia, l’encyclopédie libre

   .. raw:: html

      </title>



   Note that only the allowed Wikipedia sites for ``sleep-canada``
   service account are allowed, namely Wikipedia in English and French.

Cleanup of monitoring and policy enforcement of SNI and source identity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sh

      $ kubectl delete service sleep-us sleep-canada $
kubectl delete deployment sleep-us sleep-canada $ kubectl delete
serviceaccount sleep-us sleep-canada $ kubectl delete -f
@samples/sleep/telemetry/sni-logging.yaml@ $ kubectl delete -f
@samples/sleep/policy/sni-serviceaccount.yaml@

Cleanup
-------

1. Perform `the cleanup
   steps </docs/tasks/traffic-management/egress/wildcard-egress-hosts/#cleanup-wildcard-configuration-for-arbitrary-domains>`_
   from `Configure Egress Traffic using Wildcard
   Hosts </docs/tasks/traffic-management/egress/wildcard-egress-hosts/>`_
   example.

2. Shutdown the
   `sleep <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/sleep>`_
   service:

   .. code:: sh

      $ kubectl delete -f @samples/sleep/sleep.yaml@
