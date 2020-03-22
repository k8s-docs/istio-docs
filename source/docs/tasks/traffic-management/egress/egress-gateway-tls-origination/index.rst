Egress Gateways with TLS Origination
============================================================

The `TLS Origination for Egress
Traffic </docs/tasks/traffic-management/egress/egress-tls-origination/>`_
example shows how to configure Istio to perform {{< gloss >}}TLS
origination{{< /gloss >}} for traffic to an external service. The
`Configure an Egress
Gateway </docs/tasks/traffic-management/egress/egress-gateway/>`_
example shows how to configure Istio to direct egress traffic through a
dedicated *egress gateway* service. This example combines the previous
two by describing how to configure an egress gateway to perform TLS
origination for traffic to external services.

Before you begin
----------------

-  Setup Istio by following the instructions in the `Installation
   guide </docs/setup/>`_.

-  Start the
   `sleep <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/sleep>`_ sample
   which will be used as a test source for external calls.

   If you have enabled `automatic sidecar
   injection </docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection>`_,
   do

   .. code:: sh

      $ kubectl apply -f @samples/sleep/sleep.yaml@

   otherwise, you have to manually inject the sidecar before deploying
   the ``sleep`` application:

   .. code:: sh

      $ kubectl apply -f <(istioctl kube-inject -f
   @samples/sleep/sleep.yaml@)

   Note that any pod that you can ``exec`` and ``curl`` from would do.

-  Create a shell variable to hold the name of the source pod for
   sending requests to external services. If you used the
   `sleep <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/sleep>`_ sample,
   run:

   .. code:: sh

      $ export SOURCE_POD=$(kubectl get pod -l app=sleep
   -o jsonpath={.items..metadata.name})

-  `Deploy Istio egress
   gateway </docs/tasks/traffic-management/egress/egress-gateway/#deploy-istio-egress-gateway>`_.

-  `Enable Envoy’s access
   logging </docs/tasks/observability/logs/access-log/#enable-envoy-s-access-logging>`_

Perform TLS origination with an egress gateway
----------------------------------------------

This section describes how to perform the same TLS origination as in the
`TLS Origination for Egress
Traffic </docs/tasks/traffic-management/egress/egress-tls-origination/>`_
example, only this time using an egress gateway. Note that in this case
the TLS origination will be done by the egress gateway, as opposed to by
the sidecar in the previous example.

1. Define a ``ServiceEntry`` for ``edition.cnn.com``:

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   networking.istio.io/v1alpha3 kind: ServiceEntry metadata: name: cnn
   spec: hosts:

   -  edition.cnn.com ports:
   -  number: 80 name: http protocol: HTTP
   -  number: 443 name: https protocol: HTTPS resolution: DNS EOF

2. Verify that your ``ServiceEntry`` was applied correctly by sending a
   request to
   `http://edition.cnn.com/politics <https://edition.cnn.com/politics>`_.

   .. code:: sh

      $ kubectl exec -it $SOURCE_POD -c sleep – curl -sL
   -o /dev/null -D - http://edition.cnn.com/politics HTTP/1.1 301 Moved
   Permanently … location: https://edition.cnn.com/politics …

   command terminated with exit code 35

   Your ``ServiceEntry`` was configured correctly if you see *301 Moved
   Permanently* in the output.

3. Create an egress ``Gateway`` for *edition.cnn.com*, port 443, and a
   destination rule for sidecar requests that will be directed to the
   egress gateway.

   Choose the instructions corresponding to whether or not you want to
   enable `mutual TLS
   Authentication </docs/tasks/security/authentication/authn-policy/>`_
   between the source pod and the egress gateway.

   .. note::

   You may want to enable mutual TLS so the traffic between
   the source pod and the egress gateway will be encrypted. In addition,
   mutual TLS will allow the egress gateway to monitor the identity of
   the source pods and enable Mixer policy enforcement based on that
   identity.

   {{< tabset category-name=“mtls” >}}

   {{< tab name=“mutual TLS enabled” category-value=“enabled” >}}

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   networking.istio.io/v1alpha3 kind: Gateway metadata: name:
   istio-egressgateway spec: selector: istio: egressgateway servers:

   -  port: number: 80 name: https protocol: HTTPS hosts:

      -  edition.cnn.com tls: mode: MUTUAL serverCertificate:
         /etc/certs/cert-chain.pem privateKey: /etc/certs/key.pem
         caCertificates: /etc/certs/root-cert.pem — apiVersion:
         networking.istio.io/v1alpha3 kind: DestinationRule metadata:
         name: egressgateway-for-cnn spec: host:
         istio-egressgateway.istio-system.svc.cluster.local subsets:

   -  name: cnn trafficPolicy: loadBalancer: simple: ROUND_ROBIN
      portLevelSettings:

      -  port: number: 80 tls: mode: ISTIO_MUTUAL sni: edition.cnn.com
         EOF

   {{< /tab >}}

   {{< tab name=“mutual TLS disabled” category-value=“disabled” >}}

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   networking.istio.io/v1alpha3 kind: Gateway metadata: name:
   istio-egressgateway spec: selector: istio: egressgateway servers:

   -  port: number: 80 name: http-port-for-tls-origination protocol:
      HTTP hosts:

      -  .. rubric:: edition.cnn.com
            :name: edition.cnn.com

         apiVersion: networking.istio.io/v1alpha3 kind: DestinationRule
         metadata: name: egressgateway-for-cnn spec: host:
         istio-egressgateway.istio-system.svc.cluster.local subsets:

   -  name: cnn EOF

   {{< /tab >}}

   {{< /tabset >}}

4. Define a ``VirtualService`` to direct the traffic through the egress
   gateway, and a ``DestinationRule`` to perform TLS origination for
   requests to ``edition.cnn.com``:

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   networking.istio.io/v1alpha3 kind: VirtualService metadata: name:
   direct-cnn-through-egress-gateway spec: hosts:

   -  edition.cnn.com gateways:
   -  istio-egressgateway
   -  mesh http:
   -  match:

      -  gateways:

         -  mesh port: 80 route:

      -  destination: host:
         istio-egressgateway.istio-system.svc.cluster.local subset: cnn
         port: number: 80 weight: 100

   -  match:

      -  gateways:

         -  istio-egressgateway port: 80 route:

      -  destination: host: edition.cnn.com port: number: 443 weight:
         100 — apiVersion: networking.istio.io/v1alpha3 kind:
         DestinationRule metadata: name:
         originate-tls-for-edition-cnn-com spec: host: edition.cnn.com
         trafficPolicy: loadBalancer: simple: ROUND_ROBIN
         portLevelSettings:
      -  port: number: 443 tls: mode: SIMPLE # initiates HTTPS for
         connections to edition.cnn.com EOF

5. Send an HTTP request to
   `http://edition.cnn.com/politics <https://edition.cnn.com/politics>`_.

   .. code:: sh

      $ kubectl exec -it $SOURCE_POD -c sleep – curl -sL
   -o /dev/null -D - http://edition.cnn.com/politics HTTP/1.1 200 OK …
   content-length: 150793 …

   The output should be the same as in the `TLS Origination for Egress
   Traffic </docs/tasks/traffic-management/egress/egress-tls-origination/>`_
   example, with TLS origination: without the *301 Moved Permanently*
   message.

6. Check the log of the ``istio-egressgateway`` pod and you should see a
   line corresponding to our request. If Istio is deployed in the
   ``istio-system`` namespace, the command to print the log is:

   .. code:: sh

      $ kubectl logs -l istio=egressgateway -c
   istio-proxy -n istio-system \| tail

   You should see a line similar to the following:

   {{< text plain>}} “[2018-06-14T13:49:36.340Z]”GET /politics HTTP/1.1"
   200 - 0 148528 5096 90 “172.30.146.87” “curl/7.35.0”
   “c6bfdfc3-07ec-9c30-8957-6904230fd037” “edition.cnn.com”
   “151.101.65.67:443”

Cleanup the TLS origination example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove the Istio configuration items you created:

.. code:: sh

      $ kubectl delete gateway istio-egressgateway $ kubectl
delete serviceentry cnn $ kubectl delete virtualservice
direct-cnn-through-egress-gateway $ kubectl delete destinationrule
originate-tls-for-edition-cnn-com $ kubectl delete destinationrule
egressgateway-for-cnn

Perform mutual TLS origination with an egress gateway
-----------------------------------------------------

Similar to the previous section, this section describes how to configure
an egress gateway to perform TLS origination for an external service,
only this time using a service that requires mutual TLS.

This example is considerably more involved because you need to first:

1. generate client and server certificates
2. deploy an external service that supports the mutual TLS protocol
3. redeploy the egress gateway with the needed mutual TLS certs

Only then can you configure the external traffic to go through the
egress gateway which will perform TLS origination.

Generate client and server certificates and keys
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Clone the https://github.com/nicholasjackson/mtls-go-example
   repository:

   .. code:: sh

      $ git clone
   https://github.com/nicholasjackson/mtls-go-example

2. Change directory to the cloned repository:

   .. code:: sh

      $ cd mtls-go-example

3. Generate the certificates for ``nginx.example.com``. Use any password
   with the following command:

   .. code:: sh

      $ ./generate.sh nginx.example.com

   Select ``y`` for all prompts that appear.

4. Move the certificates into the ``nginx.example.com`` directory:

   .. code:: sh

      $ mkdir ../nginx.example.com && mv 1_root
   2_intermediate 3_application 4_client ../nginx.example.com {{< /text
   >}}

5. Go back to your previous directory:

   .. code:: sh

      $ cd ..

Deploy a mutual TLS server
~~~~~~~~~~~~~~~~~~~~~~~~~~

To simulate an actual external service that supports the mutual TLS
protocol, deploy an `NGINX <https://www.nginx.com>`_ server in your
Kubernetes cluster, but running outside of the Istio service mesh, i.e.,
in a namespace without Istio sidecar proxy injection enabled.

1. Create a namespace to represent services outside the Istio mesh,
   namely ``mesh-external``. Note that the sidecar proxy will not be
   automatically injected into the pods in this namespace since the
   automatic sidecar injection was not
   `enabled </docs/setup/additional-setup/sidecar-injection/#deploying-an-app>`_
   on it.

   .. code:: sh

      $ kubectl create namespace mesh-external {{< /text
   >}}

2. Create Kubernetes
   `Secrets <https://kubernetes.io/docs/concepts/configuration/secret/>`_
   to hold the server’s and CA certificates.

   .. code:: sh

      $ kubectl create -n mesh-external secret tls
   nginx-server-certs –key
   nginx.example.com/3_application/private/nginx.example.com.key.pem
   –cert
   nginx.example.com/3_application/certs/nginx.example.com.cert.pem $
   kubectl create -n mesh-external secret generic nginx-ca-certs
   –from-file=nginx.example.com/2_intermediate/certs/ca-chain.cert.pem


3. Create a configuration file for the NGINX server:

   .. code:: sh

      $ cat < ./nginx.conf events { }

   http { log_format main ‘$remote_addr -
   :math:`remote_user [`\ time_local] :math:`status '  '"`\ request"
   :math:`body_bytes_sent "`\ http_referer"’
   ‘“:math:`http_user_agent" "`\ http_x_forwarded_for”’; access_log
   /var/log/nginx/access.log main; error_log /var/log/nginx/error.log;

   server { listen 443 ssl;

   ::

      root /usr/share/nginx/html;
      index index.html;

      server_name nginx.example.com;
      ssl_certificate /etc/nginx-server-certs/tls.crt;
      ssl_certificate_key /etc/nginx-server-certs/tls.key;
      ssl_client_certificate /etc/nginx-ca-certs/ca-chain.cert.pem;
      ssl_verify_client on;

   } } EOF

4. Create a Kubernetes
   `ConfigMap <https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/>`_
   to hold the configuration of the NGINX server:

   .. code:: sh

      $ kubectl create configmap nginx-configmap -n
   mesh-external –from-file=nginx.conf=./nginx.conf

5. Deploy the NGINX server:

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion: v1 kind:
   Service metadata: name: my-nginx namespace: mesh-external labels:
   run: my-nginx spec: ports:

   -  port: 443 protocol: TCP selector: run: my-nginx — apiVersion:
      apps/v1 kind: Deployment metadata: name: my-nginx namespace:
      mesh-external spec: selector: matchLabels: run: my-nginx replicas:
      1 template: metadata: labels: run: my-nginx spec: containers:

      -  name: my-nginx image: nginx ports:

         -  containerPort: 443 volumeMounts:
         -  name: nginx-config mountPath: /etc/nginx readOnly: true
         -  name: nginx-server-certs mountPath: /etc/nginx-server-certs
            readOnly: true
         -  name: nginx-ca-certs mountPath: /etc/nginx-ca-certs
            readOnly: true volumes:

      -  name: nginx-config configMap: name: nginx-configmap
      -  name: nginx-server-certs secret: secretName: nginx-server-certs
      -  name: nginx-ca-certs secret: secretName: nginx-ca-certs EOF

6. Define a ``ServiceEntry`` and a ``VirtualService`` for
   ``nginx.example.com`` to instruct Istio to direct traffic destined to
   ``nginx.example.com`` to your NGINX server:

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   networking.istio.io/v1alpha3 kind: ServiceEntry metadata: name: nginx
   spec: hosts:

   -  nginx.example.com ports:
   -  number: 80 name: http protocol: HTTP
   -  number: 443 name: https protocol: HTTPS resolution: DNS endpoints:
   -  address: my-nginx.mesh-external.svc.cluster.local ports: https:
      443 — apiVersion: networking.istio.io/v1alpha3 kind:
      VirtualService metadata: name: nginx spec: hosts:
   -  nginx.example.com tls:
   -  match:

      -  port: 443 sni_hosts:

         -  nginx.example.com route:

      -  destination: host: nginx.example.com port: number: 443 weight:
         100 EOF

Deploy a container to test the NGINX deployment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Create Kubernetes
   `Secrets <https://kubernetes.io/docs/concepts/configuration/secret/>`_
   to hold the client’s and CA certificates:

   .. code:: sh

      $ kubectl create secret tls nginx-client-certs –key
   nginx.example.com/4_client/private/nginx.example.com.key.pem –cert
   nginx.example.com/4_client/certs/nginx.example.com.cert.pem $ kubectl
   create secret generic nginx-ca-certs
   –from-file=nginx.example.com/2_intermediate/certs/ca-chain.cert.pem


2. Deploy the
   `sleep <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/sleep>`_ sample
   with mounted client and CA certificates to test sending requests to
   the NGINX server:

   .. code:: sh

      $ kubectl apply -f - <<EOF # Copyright 2017 Istio
   Authors # # Licensed under the Apache License, Version 2.0 (the
   “License”); # you may not use this file except in compliance with the
   License. # You may obtain a copy of the License at # #
   http://www.apache.org/licenses/LICENSE-2.0 # # Unless required by
   applicable law or agreed to in writing, software # distributed under
   the License is distributed on an “AS IS” BASIS, # WITHOUT WARRANTIES
   OR CONDITIONS OF ANY KIND, either express or implied. # See the
   License for the specific language governing permissions and #
   limitations under the License.

   .. rubric::
      :name: section

   .. rubric:: Sleep service
      :name: sleep-service

   .. rubric::
      :name: section-1

   apiVersion: v1 kind: Service metadata: name: sleep labels: app: sleep
   spec: ports:

   -  port: 80 name: http selector: app: sleep — apiVersion: apps/v1
      kind: Deployment metadata: name: sleep spec: replicas: 1 template:
      metadata: labels: app: sleep spec: containers:

      -  name: sleep image: tutum/curl command:
         [“/bin/sleep”,“infinity”] imagePullPolicy: IfNotPresent
         volumeMounts:

         -  name: nginx-client-certs mountPath: /etc/nginx-client-certs
            readOnly: true
         -  name: nginx-ca-certs mountPath: /etc/nginx-ca-certs
            readOnly: true volumes:

      -  name: nginx-client-certs secret: secretName: nginx-client-certs
      -  name: nginx-ca-certs secret: secretName: nginx-ca-certs EOF

3. Define an environment variable to hold the name of the ``sleep`` pod:

   .. code:: sh

      $ export SOURCE_POD=$(kubectl get pod -l app=sleep
   -o jsonpath={.items..metadata.name})

4. Use the deployed
   `sleep <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/sleep>`_ pod to
   send requests to the NGINX server. Since ``nginx.example.com`` does
   not actually exist and therefore DNS cannot resolve it, the following
   ``curl`` command uses the ``--resolve`` option to resolve the
   hostname manually. The IP value passed in the –resolve option
   (1.1.1.1 below) is not significant. Any value other than 127.0.0.1
   can be used. Normally, a DNS entry exists for the destination
   hostname and you would not use the ``--resolve`` option of ``curl``.

   .. code:: sh

      $ kubectl exec -it $SOURCE_POD -c sleep – curl -v
   –resolve nginx.example.com:443:1.1.1.1 –cacert
   /etc/nginx-ca-certs/ca-chain.cert.pem –cert
   /etc/nginx-client-certs/tls.crt –key /etc/nginx-client-certs/tls.key
   https://nginx.example.com … Server certificate: subject: C=US;
   ST=Denial; L=Springfield; O=Dis; CN=nginx.example.com start date:
   2018-08-16 04:31:20 GMT expire date: 2019-08-26 04:31:20 GMT common
   name: nginx.example.com (matched) issuer: C=US; ST=Denial; O=Dis;
   CN=nginx.example.com SSL certificate verify ok. > GET / HTTP/1.1 >
   User-Agent: curl/7.35.0 > Host: nginx.example.com … < HTTP/1.1 200 OK

   < Server: nginx/1.15.2 … <!DOCTYPE html>

   .. raw:: html

      <html>

   .. raw:: html

      <head>

   .. raw:: html

      <title>

   Welcome to nginx!

   .. raw:: html

      </title>

   …

5. Verify that the server requires the client’s certificate:

   .. code:: sh

      $ kubectl exec -it $(kubectl get pod -l app=sleep
   -o jsonpath={.items..metadata.name}) -c sleep – curl -k –resolve
   nginx.example.com:443:1.1.1.1 https://nginx.example.com

   .. raw:: html

      <html>

   .. raw:: html

      <head>

   .. raw:: html

      <title>

   400 No required SSL certificate was sent

   .. raw:: html

      </title>

   .. raw:: html

      </head>

   .. raw:: html

      <body bgcolor="white">

   .. raw:: html

      <center>

   .. raw:: html

      <h1>

   400 Bad Request

   .. raw:: html

      </h1>

   .. raw:: html

      </center>

   .. raw:: html

      <center>

   No required SSL certificate was sent

   .. raw:: html

      </center>

   .. raw:: html

      <hr>

   .. raw:: html

      <center>

   nginx/1.15.2

   .. raw:: html

      </center>

   .. raw:: html

      </body>

   .. raw:: html

      </html>



Redeploy the egress gateway with the client certificates
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Create Kubernetes
   `Secrets <https://kubernetes.io/docs/concepts/configuration/secret/>`_
   to hold the client’s and CA certificates.

   .. code:: sh

      $ kubectl create -n istio-system secret tls
   nginx-client-certs –key
   nginx.example.com/4_client/private/nginx.example.com.key.pem –cert
   nginx.example.com/4_client/certs/nginx.example.com.cert.pem $ kubectl
   create -n istio-system secret generic nginx-ca-certs
   –from-file=nginx.example.com/2_intermediate/certs/ca-chain.cert.pem


2. Generate the ``istio-egressgateway`` deployment with a volume to be
   mounted from the new secrets. Use the same options you used for
   generating your ``istio.yaml``:

   | .. code:: sh

      $ istioctl manifest generate –set
     values.gateways.istio-ingressgateway.enabled=false
   | –set values.gateways.istio-egressgateway.enabled=true
   | –set
     ‘values.gateways.istio-egressgateway.secretVolumes[0].name’=egressgateway-certs
   | –set
     ‘values.gateways.istio-egressgateway.secretVolumes[0].secretName’=istio-egressgateway-certs
   | –set
     ‘values.gateways.istio-egressgateway.secretVolumes[0].mountPath’=/etc/istio/egressgateway-certs
   | –set
     ‘values.gateways.istio-egressgateway.secretVolumes[1].name’=egressgateway-ca-certs
   | –set
     ‘values.gateways.istio-egressgateway.secretVolumes[1].secretName’=istio-egressgateway-ca-certs
   | –set
     ‘values.gateways.istio-egressgateway.secretVolumes[1].mountPath’=/etc/istio/egressgateway-ca-certs
   | –set
     ‘values.gateways.istio-egressgateway.secretVolumes[2].name’=nginx-client-certs
   | –set
     ‘values.gateways.istio-egressgateway.secretVolumes[2].secretName’=nginx-client-certs
   | –set
     ‘values.gateways.istio-egressgateway.secretVolumes[2].mountPath’=/etc/nginx-client-certs
   | –set
     ‘values.gateways.istio-egressgateway.secretVolumes[3].name’=nginx-ca-certs
   | –set
     ‘values.gateways.istio-egressgateway.secretVolumes[3].secretName’=nginx-ca-certs
   | –set
     ‘values.gateways.istio-egressgateway.secretVolumes[3].mountPath’=/etc/nginx-ca-certs
     >
   | ./istio-egressgateway.yaml

3. Redeploy ``istio-egressgateway``:

   .. code:: sh

      $ kubectl apply -f ./istio-egressgateway.yaml
   deployment “istio-egressgateway” configured

4. Verify that the key and the certificate are successfully loaded in
   the ``istio-egressgateway`` pod:

   .. code:: sh

      $ kubectl exec -it -n istio-system $(kubectl -n
   istio-system get pods -l istio=egressgateway -o
   jsonpath=‘{.items[0].metadata.name}’) – ls -al
   /etc/nginx-client-certs /etc/nginx-ca-certs

   ``tls.crt`` and ``tls.key`` should exist in
   ``/etc/istio/nginx-client-certs``, while ``ca-chain.cert.pem`` in
   ``/etc/istio/nginx-ca-certs``.

Configure mutual TLS origination for egress traffic
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Create an egress ``Gateway`` for ``nginx.example.com``, port 443, and
   destination rules and virtual services to direct the traffic through
   the egress gateway and from the egress gateway to the external
   service.

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   networking.istio.io/v1alpha3 kind: Gateway metadata: name:
   istio-egressgateway spec: selector: istio: egressgateway servers:

   -  port: number: 443 name: https protocol: HTTPS hosts:

      -  nginx.example.com tls: mode: MUTUAL serverCertificate:
         /etc/certs/cert-chain.pem privateKey: /etc/certs/key.pem
         caCertificates: /etc/certs/root-cert.pem — apiVersion:
         networking.istio.io/v1alpha3 kind: DestinationRule metadata:
         name: egressgateway-for-nginx spec: host:
         istio-egressgateway.istio-system.svc.cluster.local subsets:

   -  name: nginx trafficPolicy: loadBalancer: simple: ROUND_ROBIN
      portLevelSettings:

      -  port: number: 443 tls: mode: ISTIO_MUTUAL sni:
         nginx.example.com EOF

2. Define a ``VirtualService`` to direct the traffic through the egress
   gateway, and a ``DestinationRule`` to perform mutual TLS origination:

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   networking.istio.io/v1alpha3 kind: VirtualService metadata: name:
   direct-nginx-through-egress-gateway spec: hosts:

   -  nginx.example.com gateways:
   -  istio-egressgateway
   -  mesh http:
   -  match:

      -  gateways:

         -  mesh port: 80 route:

      -  destination: host:
         istio-egressgateway.istio-system.svc.cluster.local subset:
         nginx port: number: 443 weight: 100

   -  match:

      -  gateways:

         -  istio-egressgateway port: 443 route:

      -  destination: host: nginx.example.com port: number: 443 weight:
         100 — apiVersion: networking.istio.io/v1alpha3 kind:
         DestinationRule metadata: name: originate-mtls-for-nginx spec:
         host: nginx.example.com trafficPolicy: loadBalancer: simple:
         ROUND_ROBIN portLevelSettings:
      -  port: number: 443 tls: mode: MUTUAL clientCertificate:
         /etc/nginx-client-certs/tls.crt privateKey:
         /etc/nginx-client-certs/tls.key caCertificates:
         /etc/nginx-ca-certs/ca-chain.cert.pem sni: nginx.example.com
         EOF

3. Send an HTTP request to ``http://nginx.example.com``:

   .. code:: sh

      $ kubectl exec -it $SOURCE_POD -c sleep – curl -s
   –resolve nginx.example.com:80:1.1.1.1 http://nginx.example.com
   <!DOCTYPE html>

   .. raw:: html

      <html>

   .. raw:: html

      <head>

   .. raw:: html

      <title>

   Welcome to nginx!

   .. raw:: html

      </title>

   …

4. Check the log of the ``istio-egressgateway`` pod for a line
   corresponding to our request. If Istio is deployed in the
   ``istio-system`` namespace, the command to print the log is:

   .. code:: sh

      $ kubectl logs -l istio=egressgateway -n
   istio-system \| grep ‘nginx.example.com’ \| grep HTTP

   You should see a line similar to the following:

   {{< text plain>}} [2018-08-19T18:20:40.096Z] “GET / HTTP/1.1” 200 - 0
   612 7 5 “172.30.146.114” “curl/7.35.0”
   “b942b587-fac2-9756-8ec6-303561356204” “nginx.example.com”
   “172.21.72.197:443”

Cleanup the mutual TLS origination example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Remove created Kubernetes resources:

   .. code:: sh

      $ kubectl delete secret nginx-server-certs
   nginx-ca-certs -n mesh-external $ kubectl delete secret
   nginx-client-certs nginx-ca-certs $ kubectl delete secret
   nginx-client-certs nginx-ca-certs -n istio-system $ kubectl delete
   configmap nginx-configmap -n mesh-external $ kubectl delete service
   my-nginx -n mesh-external $ kubectl delete deployment my-nginx -n
   mesh-external $ kubectl delete namespace mesh-external $ kubectl
   delete gateway istio-egressgateway $ kubectl delete serviceentry
   nginx $ kubectl delete virtualservice
   direct-nginx-through-egress-gateway $ kubectl delete destinationrule
   originate-mtls-for-nginx $ kubectl delete destinationrule
   egressgateway-for-nginx

2. Delete the directory of certificates and the repository used to
   generate them:

   .. code:: sh

      $ rm -rf nginx.example.com mtls-go-example

3. Delete the generated configuration files used in this example:

   .. code:: sh

      $ rm -f ./nginx.conf ./istio-egressgateway.yaml

Cleanup
-------

Delete the ``sleep`` service and deployment:

.. code:: sh

      $ kubectl delete service sleep $ kubectl delete
deployment sleep
