custom-ingress-gateway
================================================

This post provides instructions to manually create a custom ingress
`gateway </docs/reference/config/networking/gateway/>`_ with automatic
provisioning of certificates based on cert-manager.

The creation of custom ingress gateway could be used in order to have
different ``loadbalancer`` in order to isolate traffic.

Before you begin
----------------

-  Setup Istio by following the instructions in the `Installation
   guide </docs/setup/>`_.
-  Setup ``cert-manager`` with helm
   `chart <https://github.com/helm/charts/tree/master/stable/cert-manager#installing-the-chart>`_
-  We will use ``demo.mydemo.com`` for our example, it must be resolved
   with your DNS

Configuring the custom ingress gateway
--------------------------------------

1.  Check if
    `cert-manager <https://github.com/helm/charts/tree/master/stable/cert-manager>`_
    was installed using Helm with the following command:

    .. code:: sh

      $ helm ls

    The output should be similar to the example below and show
    cert-manager with a ``STATUS`` of ``DEPLOYED``:

    {{< text plain >}} NAME REVISION UPDATED STATUS CHART APP VERSION
    NAMESPACE istio 1 Thu Oct 11 13:34:24 2018 DEPLOYED istio-1.0.X
    1.0.X istio-system cert 1 Wed Oct 24 14:08:36 2018 DEPLOYED
    cert-manager-v0.6.0-dev.2 v0.6.0-dev.2 istio-system

2.  To create the cluster’s issuer, apply the following configuration:

    .. note::

   Change the cluster’s
    `issuer <https://cert-manager.readthedocs.io/en/latest/reference/issuers.html>`_
    provider with your own configuration values. The example uses the
    values under ``route53``.

    .. code:: yaml

    apiVersion: certmanager.k8s.io/v1alpha1 kind:
    ClusterIssuer metadata: name: letsencrypt-demo namespace:
    kube-system spec: acme: # The ACME server URL server:
    https://acme-v02.api.letsencrypt.org/directory # Email address used
    for ACME registration email: # Name of a secret used to store the
    ACME account private key privateKeySecretRef: name: letsencrypt-demo
    dns01: # Here we define a list of DNS-01 providers that can solve
    DNS challenges providers: - name: your-dns route53: accessKeyID:
    region: eu-central-1 secretAccessKeySecretRef: name:
    prod-route53-credentials-secret key: secret-access-key

3.  If you use the ``route53``
    `provider <https://cert-manager.readthedocs.io/en/latest/tasks/acme/configuring-dns01/route53.html>`_,
    you must provide a secret to perform DNS ACME Validation. To create
    the secret, apply the following configuration file:

    .. code:: yaml

    apiVersion: v1 kind: Secret metadata: name:
    prod-route53-credentials-secret type: Opaque data:
    secret-access-key:

4.  Create your own certificate:

    .. code:: yaml

    apiVersion: certmanager.k8s.io/v1alpha1 kind:
    Certificate metadata: name: demo-certificate namespace: istio-system
    spec: acme: config: - dns01: provider: your-dns domains: -
    ‘*.mydemo.com’ commonName: ’*.mydemo.com’ dnsNames:

    -  ’*.mydemo.com’ issuerRef: kind: ClusterIssuer name:
       letsencrypt-demo secretName: istio-customingressgateway-certs

    Make a note of the value of ``secretName`` since a future step
    requires it.

5.  To scale automatically, declare a new horizontal pod autoscaler with
    the following configuration:

    .. code:: yaml

    apiVersion: autoscaling/v1 kind:
    HorizontalPodAutoscaler metadata: name: my-ingressgateway namespace:
    istio-system spec: maxReplicas: 5 minReplicas: 1 scaleTargetRef:
    apiVersion: apps/v1beta1 kind: Deployment name: my-ingressgateway
    targetCPUUtilizationPercentage: 80 status:
    currentCPUUtilizationPercentage: 0 currentReplicas: 1
    desiredReplicas: 1

6.  Apply your deployment with declaration provided in the `yaml
    definition </blog/2019/custom-ingress-gateway/deployment-custom-ingress.yaml>`_

    .. note::

   The annotations used, for example
    ``aws-load-balancer-type``, only apply for AWS.

7.  Create your service:

    .. warning::

   The ``NodePort`` used needs to be an available port.


    .. code:: yaml

    apiVersion: v1 kind: Service metadata: name:
    my-ingressgateway annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: nlb labels: app:
    my-ingressgateway istio: my-ingressgateway spec: type: LoadBalancer
    selector: app: my-ingressgateway istio: my-ingressgateway ports: -
    name: http2 nodePort: 32380 port: 80 targetPort: 80 - name: https
    nodePort: 32390 port: 443 - name: tcp nodePort: 32400 port: 31400


8.  Create your Istio custom gateway configuration object:

    .. code:: yaml

    apiVersion: networking.istio.io/v1alpha3 kind:
    Gateway metadata: annotations: name: istio-custom-gateway namespace:
    default spec: selector: istio: my-ingressgateway servers:

    -  hosts:

       -  ’*.mydemo.com’ port: name: http number: 80 protocol: HTTP tls:
          httpsRedirect: true

    -  hosts:

       -  ’*.mydemo.com’ port: name: https number: 443 protocol: HTTPS
          tls: mode: SIMPLE privateKey:
          /etc/istio/ingressgateway-certs/tls.key serverCertificate:
          /etc/istio/ingressgateway-certs/tls.crt

9.  Link your ``istio-custom-gateway`` with your ``VirtualService``:

    .. code:: yaml

    apiVersion: networking.istio.io/v1alpha3 kind:
    VirtualService metadata: name: my-virtualservice spec: hosts:

    -  “demo.mydemo.com” gateways:
    -  istio-custom-gateway http:
    -  route:

       -  destination: host: my-demoapp

10. Correct certificate is returned by the server and it is successfully
    verified (*SSL certificate verify ok* is printed):

    .. code:: sh

      $ curl -v ``https://demo.mydemo.com`` Server
    certificate: SSL certificate verify ok.

**Congratulations!** You can now use your custom
``istio-custom-gateway``
`gateway </docs/reference/config/networking/gateway/>`_ configuration
object.
