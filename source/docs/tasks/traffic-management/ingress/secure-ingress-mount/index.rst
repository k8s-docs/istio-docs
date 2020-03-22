Secure Gateways (File Mount)
============================================================================

The `Control Ingress Traffic
task </docs/tasks/traffic-management/ingress>`_ describes how to
configure an ingress gateway to expose an HTTP service to external
traffic. This task shows how to expose a secure HTTPS service using
either simple or mutual TLS.

The TLS required private key, server certificate, and root certificate,
are configured using a file mount based approach.

Before you begin
----------------

1. Perform the steps in the `Before you
   begin </docs/tasks/traffic-management/ingress/ingress-control#before-you-begin>`_
   and `Determining the ingress IP and
   ports </docs/tasks/traffic-management/ingress/ingress-control/#determining-the-ingress-ip-and-ports>`_
   sections of the `Control Ingress
   Traffic </docs/tasks/traffic-management/ingress>`_ task. After
   performing those steps you should have Istio and the
   `httpbin <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/httpbin>`_
   service deployed, and the environment variables ``INGRESS_HOST`` and
   ``SECURE_INGRESS_PORT`` set.

2. For macOS users, verify that you use *curl* compiled with the
   `LibreSSL <http://www.libressl.org>`_ library:

   .. code:: sh

      $ curl –version \| grep LibreSSL curl 7.54.0
   (x86_64-apple-darwin17.0) libcurl/7.54.0 LibreSSL/2.0.20 zlib/1.2.11
   nghttp2/1.24.0

   If a version of *LibreSSL* is printed as in the output above, your
   *curl* should work correctly with the instructions in this task.
   Otherwise, try another installation of *curl*, for example on a Linux
   machine.

Generate server certificate and private key
-------------------------------------------

For this task you can use your favorite tool to generate certificates
and keys. The commands below use
`openssl <https://man.openbsd.org/openssl.1>`_

1. Create a root certificate and private key to sign the certificate for
   your services:

   .. code:: sh

      $ openssl req -x509 -sha256 -nodes -days 365
   -newkey rsa:2048 -subj ‘/O=example Inc./CN=example.com’ -keyout
   example.com.key -out example.com.crt

2. Create a certificate and a private key for ``httpbin.example.com``:

   .. code:: sh

      $ openssl req -out httpbin.example.com.csr -newkey
   rsa:2048 -nodes -keyout httpbin.example.com.key -subj
   “/CN=httpbin.example.com/O=httpbin organization” $ openssl x509 -req
   -days 365 -CA example.com.crt -CAkey example.com.key -set_serial 0
   -in httpbin.example.com.csr -out httpbin.example.com.crt {{< /text
   >}}

Configure a TLS ingress gateway with a file mount-based approach
----------------------------------------------------------------

In this section you configure an ingress gateway with port 443 to handle
HTTPS traffic. You first create a secret with a certificate and a
private key. The secret is mounted to a file on the
``/etc/istio/ingressgateway-certs`` path. You can then create a gateway
definition that configures a server on port 443.

1. Create a Kubernetes secret to hold the server’s certificate and
   private key. Use ``kubectl`` to create the secret
   ``istio-ingressgateway-certs`` in namespace ``istio-system`` . The
   Istio gateway will load the secret automatically.

   .. warning::

   The secret **must** be named
   ``istio-ingressgateway-certs`` in the ``istio-system`` namespace to
   align with the configuration of the Istio default ingress gateway
   used in this task.

   .. code:: sh

      $ kubectl create -n istio-system secret tls
   istio-ingressgateway-certs –key httpbin.example.com.key –cert
   httpbin.example.com.crt secret “istio-ingressgateway-certs” created


   Note that by default all the pods in the ``istio-system`` namespace
   can mount this secret and access the private key. You may want to
   deploy the ingress gateway in a separate namespace and create the
   secret there, so that only the ingress gateway pod will be able to
   mount it.

   Verify that ``tls.crt`` and ``tls.key`` have been mounted in the
   ingress gateway pod:

   .. code:: sh

      $ kubectl exec -it -n istio-system $(kubectl -n
   istio-system get pods -l istio=ingressgateway -o
   jsonpath=‘{.items[0].metadata.name}’) – ls -al
   /etc/istio/ingressgateway-certs

2. Define a ``Gateway`` with a ``server`` section for port 443.

   .. warning::

   The location of the certificate and the private key
   **must** be ``/etc/istio/ingressgateway-certs``, or the gateway will
   fail to load them.

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   networking.istio.io/v1alpha3 kind: Gateway metadata: name:
   httpbin-gateway spec: selector: istio: ingressgateway # use istio
   default ingress gateway servers:

   -  port: number: 443 name: https protocol: HTTPS tls: mode: SIMPLE
      serverCertificate: /etc/istio/ingressgateway-certs/tls.crt
      privateKey: /etc/istio/ingressgateway-certs/tls.key hosts:

      -  “httpbin.example.com” EOF

3. Configure routes for traffic entering via the ``Gateway``. Define the
   same ``VirtualService`` as in the `Control Ingress
   Traffic </docs/tasks/traffic-management/ingress/ingress-control/#configuring-ingress-using-an-istio-gateway>`_
   task:

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   networking.istio.io/v1alpha3 kind: VirtualService metadata: name:
   httpbin spec: hosts:

   -  “httpbin.example.com” gateways:
   -  httpbin-gateway http:
   -  match:

      -  uri: prefix: /status
      -  uri: prefix: /delay route:
      -  destination: port: number: 8000 host: httpbin EOF

4. Access the ``httpbin`` service with HTTPS by sending an ``https``
   request using *curl* to ``SECURE_INGRESS_PORT``.

   The ``--resolve`` flag instructs *curl* to supply the
   `SNI <https://en.wikipedia.org/wiki/Server_Name_Indication>`_ value
   ``httpbin.example.com`` when accessing the gateway IP over TLS. The
   ``--cacert`` option instructs *curl* to use your generated
   certificate to verify the server.

   .. note::

   The ``-HHost:httpbin.example.com`` flag is included but
   only really needed if ``SECURE_INGRESS_PORT`` is different from the
   actual gateway port (443), for example, if you are accessing the
   server via a mapped ``NodePort``.

   By sending the request to the ``/status/418`` URL path, you get a
   nice visual clue that your ``httpbin`` service was indeed accessed.
   The ``httpbin`` service will return the `418 I’m a
   Teapot <https://tools.ietf.org/html/rfc7168#section-2.3.3>`_ code.

   .. code:: sh

      $ curl -v -HHost:httpbin.example.com –resolve
   httpbin.example.com:\ :math:`SECURE_INGRESS_PORT:`\ INGRESS_HOST
   –cacert example.com.crt
   https://httpbin.example.com:$SECURE_INGRESS_PORT/status/418 … Server
   certificate: subject: CN=httpbin.example.com; O=httpbin organization
   start date: Oct 27 19:32:48 2019 GMT expire date: Oct 26 19:32:48
   2020 GMT common name: httpbin.example.com (matched) issuer: O=example
   Inc.; CN=example.com SSL certificate verify ok. SSL certificate
   verify ok. … HTTP/2 418 … -=[ teapot ]=-

   *….* .’ \_ \_ ``. | ."`` ^
   ``". _, \_;``\ “—”\ ``|//   |       ;/   \_     _/``"""\` {{< /text
   >}}

   .. note::

   It might take time for the gateway definition to
   propagate so you might get the following error:
   ``Failed to connect to httpbin.example.com port <your secure port>: Connection refused``.
   Wait for a minute and then retry the *curl* call.

   Look for the *Server certificate* section in the *curl* output and
   specifically a line with the matched *common name*:
   ``common name: httpbin.example.com (matched)``. The line
   ``SSL certificate verify ok`` in the output indicates that the
   server’s certificate was verified successfully. If all went well, you
   should also see a returned status of 418 along with a nice drawing of
   a teapot.

Configure a mutual TLS ingress gateway
--------------------------------------

In this section you extend your gateway’s definition from the previous
section to support `mutual
TLS <https://en.wikipedia.org/wiki/Mutual_authentication>`_ between
external clients and the gateway.

1. Create a Kubernetes ``Secret`` to hold the
   `CA <https://en.wikipedia.org/wiki/Certificate_authority>`_
   certificate that the server will use to verify its clients. Create
   the secret ``istio-ingressgateway-ca-certs`` in namespace
   ``istio-system`` using ``kubectl``. The Istio gateway will
   automatically load the secret.

   .. warning::

   The secret **must** be named
   ``istio-ingressgateway-ca-certs`` in the ``istio-system`` namespace
   to align with the configuration of the Istio default ingress gateway
   used in this task.

   .. code:: sh

      $ kubectl create -n istio-system secret generic
   istio-ingressgateway-ca-certs –from-file=example.com.crt secret
   “istio-ingressgateway-ca-certs” created

2. Redefine your previous ``Gateway`` to change the TLS mode to
   ``MUTUAL`` and to specify ``caCertificates``:

   .. warning::

   The location of the certificate **must** be
   ``/etc/istio/ingressgateway-ca-certs``, or the gateway will fail to
   load them. The file (short) name of the certificate must be identical
   to the one you created the secret from, in this case
   ``example.com.crt``.

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   networking.istio.io/v1alpha3 kind: Gateway metadata: name:
   httpbin-gateway spec: selector: istio: ingressgateway # use istio
   default ingress gateway servers:

   -  port: number: 443 name: https protocol: HTTPS tls: mode: MUTUAL
      serverCertificate: /etc/istio/ingressgateway-certs/tls.crt
      privateKey: /etc/istio/ingressgateway-certs/tls.key
      caCertificates: /etc/istio/ingressgateway-ca-certs/example.com.crt
      hosts:

      -  “httpbin.example.com” EOF

3. Access the ``httpbin`` service by HTTPS as in the previous section:

   .. code:: sh

      $ curl -HHost:httpbin.example.com –resolve
   httpbin.example.com:\ :math:`SECURE_INGRESS_PORT:`\ INGRESS_HOST
   –cacert example.com.crt
   https://httpbin.example.com:$SECURE_INGRESS_PORT/status/418 curl:
   (35) error:14094410:SSL routines:SSL3_READ_BYTES:sslv3 alert
   handshake failure

   .. warning::

   It might take time for the gateway definition to
   propagate so you might still get *418*. Wait for a minute and then
   retry the *curl* call.

   This time you will get an error since the server refuses to accept
   unauthenticated requests. You need to pass *curl* a client
   certificate and your private key for signing the request.

4. Create a client certificate for the ``httpbin.example.com`` service.
   You can designate the client by the ``httpbin-client.example.com``
   URI, or use any other URI.

   .. code:: sh

      $ openssl req -out httpbin-client.example.com.csr
   -newkey rsa:2048 -nodes -keyout httpbin-client.example.com.key -subj
   “/CN=httpbin-client.example.com/O=httpbin’s client organization” $
   openssl x509 -req -days 365 -CA example.com.crt -CAkey
   example.com.key -set_serial 0 -in httpbin-client.example.com.csr -out
   httpbin-client.example.com.crt

5. Resend the previous request by *curl*, this time passing as
   parameters your client certificate (additional ``--cert`` option) and
   your private key (the ``--key`` option):

   .. code:: sh

      $ curl -HHost:httpbin.example.com –resolve
   httpbin.example.com:\ :math:`SECURE_INGRESS_PORT:`\ INGRESS_HOST
   –cacert example.com.crt –cert httpbin-client.example.com.crt –key
   httpbin-client.example.com.key
   https://httpbin.example.com:$SECURE_INGRESS_PORT/status/418

   -=[ teapot ]=-

   *….* .’ \_ \_ ``. | ."`` ^
   ``". _, \_;``\ “—”\ ``|//   |       ;/   \_     _/``"""\` {{< /text
   >}}

   This time the server performed client authentication successfully and
   you received the pretty teapot drawing again.

Configure a TLS ingress gateway for multiple hosts
--------------------------------------------------

In this section you will configure an ingress gateway for multiple
hosts, ``httpbin.example.com`` and ``bookinfo.com``. The ingress gateway
will present to clients a unique certificate corresponding to each
requested server.

Unlike the previous sections, the Istio default ingress gateway will not
work out of the box because it is only preconfigured to support one
secure host. You’ll need to first configure and redeploy the ingress
gateway server with another secret, before you can use it to handle a
second host.

Create a server certificate and private key for ``bookinfo.com``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sh

      $ openssl req -out bookinfo.com.csr -newkey rsa:2048
-nodes -keyout bookinfo.com.key -subj “/CN=bookinfo.com/O=bookinfo
organization” $ openssl x509 -req -days 365 -CA example.com.crt -CAkey
example.com.key -set_serial 0 -in bookinfo.com.csr -out bookinfo.com.crt


Redeploy ``istio-ingressgateway`` with the new certificate
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Create a new secret to hold the certificate for ``bookinfo.com``:

   .. code:: sh

      $ kubectl create -n istio-system secret tls
   istio-ingressgateway-bookinfo-certs –key bookinfo.com.key –cert
   bookinfo.com.crt secret “istio-ingressgateway-bookinfo-certs” created


2. To include a volume mounted from the new created secret, update the
   ``istio-ingressgateway`` deployment. To patch the
   ``istio-ingressgateway`` deployment, create the following
   ``gateway-patch.json`` file:

   .. code:: sh

      $ cat > gateway-patch.json <<EOF [{ “op”: “add”,
   “path”: “/spec/template/spec/containers/0/volumeMounts/0”, “value”: {
   “mountPath”: “/etc/istio/ingressgateway-bookinfo-certs”, “name”:
   “ingressgateway-bookinfo-certs”, “readOnly”: true } }, { “op”: “add”,
   “path”: “/spec/template/spec/volumes/0”, “value”: { “name”:
   “ingressgateway-bookinfo-certs”, “secret”: { “secretName”:
   “istio-ingressgateway-bookinfo-certs”, “optional”: true } } }] EOF


3. Apply ``istio-ingressgateway`` deployment patch with the following
   command:

   .. code:: sh

      $ kubectl -n istio-system patch –type=json deploy
   istio-ingressgateway -p “$(cat gateway-patch.json)”

4. Verify that the key and certificate have been successfully loaded in
   the ``istio-ingressgateway`` pod:

   .. code:: sh

      $ kubectl exec -it -n istio-system $(kubectl -n
   istio-system get pods -l istio=ingressgateway -o
   jsonpath=‘{.items[0].metadata.name}’) – ls -al
   /etc/istio/ingressgateway-bookinfo-certs

   ``tls.crt`` and ``tls.key`` should appear in the directory contents.

Configure traffic for the ``bookinfo.com`` host
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Deploy the `Bookinfo sample
   application </docs/examples/bookinfo/>`_, without a gateway:

   .. code:: sh

      $ kubectl apply -f
   @samples/bookinfo/platform/kube/bookinfo.yaml@

2. Define a gateway for ``bookinfo.com``:

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   networking.istio.io/v1alpha3 kind: Gateway metadata: name:
   bookinfo-gateway spec: selector: istio: ingressgateway # use istio
   default ingress gateway servers:

   -  port: number: 443 name: https-bookinfo protocol: HTTPS tls: mode:
      SIMPLE serverCertificate:
      /etc/istio/ingressgateway-bookinfo-certs/tls.crt privateKey:
      /etc/istio/ingressgateway-bookinfo-certs/tls.key hosts:

      -  “bookinfo.com” EOF

3. Configure the routes for ``bookinfo.com``. Define a
   ``VirtualService`` like the one in
   `samples/bookinfo/networking/bookinfo-gateway.yaml <%7B%7B%3C%20github_file%20%3E%7D%7D/samples/bookinfo/networking/bookinfo-gateway.yaml>`_:

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   networking.istio.io/v1alpha3 kind: VirtualService metadata: name:
   bookinfo spec: hosts:

   -  “bookinfo.com” gateways:
   -  bookinfo-gateway http:
   -  match:

      -  uri: exact: /productpage
      -  uri: exact: /login
      -  uri: exact: /logout
      -  uri: prefix: /api/v1/products route:
      -  destination: host: productpage port: number: 9080 EOF {{< /text
         >}}

4. Send a request to the *Bookinfo* ``productpage``:

   .. code:: sh

      $ curl -o /dev/null -s -v -w
   “%{http_code}:raw-latex:`\n`” -HHost:bookinfo.com –resolve
   bookinfo.com:\ :math:`SECURE_INGRESS_PORT:`\ INGRESS_HOST –cacert
   example.com.crt -HHost:bookinfo.com
   https://bookinfo.com:$SECURE_INGRESS_PORT/productpage … Server
   certificate: subject: CN=bookinfo.com; O=bookinfo organization start
   date: Oct 27 20:08:32 2019 GMT expire date: Oct 26 20:08:32 2020 GMT
   common name: bookinfo.com (matched) issuer: O=example Inc.;
   CN=example.com SSL certificate verify ok. … 200

5. Verify that ``httbin.example.com`` is accessible as previously. Send
   a request to it and see again the teapot you should already love:

   .. code:: sh

      $ curl -HHost:httpbin.example.com –resolve
   httpbin.example.com:\ :math:`SECURE_INGRESS_PORT:`\ INGRESS_HOST
   –cacert example.com.crt –cert httpbin-client.example.com.crt –key
   httpbin-client.example.com.key
   https://httpbin.example.com:$SECURE_INGRESS_PORT/status/418 … -=[
   teapot ]=-

   *….* .’ \_ \_ ``. | ."`` ^
   ``". _, \_;``\ “—”\ ``|//   |       ;/   \_     _/``"""\` {{< /text
   >}}

Troubleshooting
---------------

-  Inspect the values of the ``INGRESS_HOST`` and
   ``SECURE_INGRESS_PORT`` environment variables. Make sure they have
   valid values, according to the output of the following commands:

   .. code:: sh

      $ kubectl get svc -n istio-system $ echo
   INGRESS_HOST=\ :math:`INGRESS_HOST, SECURE_INGRESS_PORT=`\ SECURE_INGRESS_PORT


-  Verify that the key and the certificate are successfully loaded in
   the ``istio-ingressgateway`` pod:

   .. code:: sh

      $ kubectl exec -it -n istio-system $(kubectl -n
   istio-system get pods -l istio=ingressgateway -o
   jsonpath=‘{.items[0].metadata.name}’) – ls -al
   /etc/istio/ingressgateway-certs

   ``tls.crt`` and ``tls.key`` should exist in the directory contents.

-  If you created the ``istio-ingressgateway-certs`` secret, but the key
   and the certificate are not loaded, delete the ingress gateway pod
   and force the ingress gateway pod to restart and reload key and
   certificate.

   .. code:: sh

      $ kubectl delete pod -n istio-system -l
   istio=ingressgateway

-  Verify that the ``Subject`` is correct in the certificate of the
   ingress gateway:

   .. code:: sh

      $ kubectl exec -i -n istio-system $(kubectl get pod
   -l istio=ingressgateway -n istio-system -o
   jsonpath=‘{.items[0].metadata.name}’) – cat
   /etc/istio/ingressgateway-certs/tls.crt \| openssl x509 -text -noout
   \| grep ‘Subject:’ Subject: CN=httpbin.example.com, O=httpbin
   organization

-  Verify that the proxy of the ingress gateway is aware of the
   certificates:

   .. code:: sh

      $ kubectl exec -ti $(kubectl get po -l
   istio=ingressgateway -n istio-system -o
   jsonpath=‘{.items[0].metadata.name}’) -n istio-system – pilot-agent
   request GET certs { “ca_cert”: "“,”cert_chain“:”Certificate Path:
   /etc/istio/ingressgateway-certs/tls.crt, Serial Number: 100212, Days
   until Expiration: 370" }

-  Check the log of ``istio-ingressgateway`` for error messages:

   .. code:: sh

      $ kubectl logs -n istio-system -l
   istio=ingressgateway

-  For macOS users, verify that you use ``curl`` compiled with the
   `LibreSSL <http://www.libressl.org>`_ library, as described in the
   `Before you begin <#before-you-begin>`_ section.

Troubleshooting for mutual TLS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In addition to the steps in the previous section, perform the following:

-  Verify that the CA certificate is loaded in the
   ``istio-ingressgateway`` pod:

   .. code:: sh

      $ kubectl exec -it -n istio-system $(kubectl -n
   istio-system get pods -l istio=ingressgateway -o
   jsonpath=‘{.items[0].metadata.name}’) – ls -al
   /etc/istio/ingressgateway-ca-certs

   ``example.com.crt`` should exist in the directory contents.

-  If you created the ``istio-ingressgateway-ca-certs`` secret, but the
   CA certificate is not loaded, delete the ingress gateway pod and
   force it to reload the certificate:

   .. code:: sh

      $ kubectl delete pod -n istio-system -l
   istio=ingressgateway

-  Verify that the ``Subject`` is correct in the CA certificate of the
   ingress gateway:

   .. code:: sh

      $ kubectl exec -i -n istio-system $(kubectl get pod
   -l istio=ingressgateway -n istio-system -o
   jsonpath=‘{.items[0].metadata.name}’) – cat
   /etc/istio/ingressgateway-ca-certs/example.com.crt \| openssl x509
   -text -noout \| grep ‘Subject:’ Subject: O=example Inc.,
   CN=example.com

Cleanup
-------

1. Delete the ``Gateway`` configuration, the ``VirtualService``, and the
   secrets:

   .. code:: sh

      $ kubectl delete gateway –ignore-not-found=true
   httpbin-gateway bookinfo-gateway $ kubectl delete virtualservice
   httpbin $ kubectl delete –ignore-not-found=true -n istio-system
   secret istio-ingressgateway-certs istio-ingressgateway-ca-certs $
   kubectl delete –ignore-not-found=true virtualservice bookinfo

2. Delete the directories of the certificates and the repository used to
   generate them:

   .. code:: sh

      $ rm -rf example.com.crt example.com.key
   httpbin.example.com.crt httpbin.example.com.key
   httpbin.example.com.csr httpbin-client.example.com.crt
   httpbin-client.example.com.key httpbin-client.example.com.csr
   bookinfo.com.crt bookinfo.com.key bookinfo.com.csr

3. Remove the patch file you used for redeployment of
   ``istio-ingressgateway``:

   .. code:: sh

      $ rm -f gateway-patch.json

4. Shutdown the
   `httpbin <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/httpbin>`_
   service:

   .. code:: sh

      $ kubectl delete –ignore-not-found=true -f
   @samples/httpbin/httpbin.yaml@
