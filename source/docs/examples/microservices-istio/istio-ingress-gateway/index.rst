istio-ingress-gateway
============================================

Until now, you used a Kubernetes Ingress to access your application from
the outside. In this module, you configure the traffic to enter through
an Istio ingress gateway, in order to apply Istio control on traffic to
your microservices.

1.  Store the name of your namespace in the ``NAMESPACE`` environment
    variable. You will need it to recognize your microservices in the
    logs:

    .. code:: sh

      $ export
    NAMESPACE=\ :math:`(kubectl config view -o jsonpath="{.contexts[?(@.name == \"`\ (kubectl
    config current-context)")].context.namespace}") $ echo $NAMESPACE
    tutorial

2.  Create an environment variable for the hostname of the Istio ingress
    gateway:

    .. code:: sh

      $ export
    MY_INGRESS_GATEWAY_HOST=istio.$NAMESPACE.bookinfo.com $ echo
    $MY_INGRESS_GATEWAY_HOST istio.tutorial.bookinfo.com

3.  Configure an Istio ingress gateway:

    .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
    networking.istio.io/v1alpha3 kind: Gateway metadata: name:
    bookinfo-gateway spec: selector: istio: ingressgateway # use Istio
    default gateway implementation servers:

    -  port: number: 80 name: http protocol: HTTP hosts:

       -  .. rubric:: $MY_INGRESS_GATEWAY_HOST
             :name: my_ingress_gateway_host

          apiVersion: networking.istio.io/v1alpha3 kind: VirtualService
          metadata: name: bookinfo spec: hosts:

    -  $MY_INGRESS_GATEWAY_HOST gateways:
    -  bookinfo-gateway.$NAMESPACE.svc.cluster.local http:
    -  match:

       -  uri: exact: /productpage
       -  uri: exact: /login
       -  uri: exact: /logout
       -  uri: prefix: /static route:
       -  destination: host: productpage port: number: 9080 EOF

4.  Set ``INGRESS_HOST`` and ``INGRESS_PORT`` using the instructions in
    the `Determining the Ingress IP and
    ports </docs/tasks/traffic-management/ingress/ingress-control/#determining-the-ingress-ip-and-ports>`_
    section.

5.  Add the output of this command to your ``/etc/hosts`` file:

    .. code:: sh

      $ echo $INGRESS_HOST $MY_INGRESS_GATEWAY_HOST

6.  Access the application’s home page from the command line:

    .. code:: sh

      $ curl -s
    :math:`MY_INGRESS_GATEWAY_HOST:`\ INGRESS_PORT/productpage \| grep
    -o "

    .. raw:: html

       <title>

    .\*

    .. raw:: html

       </title>

    "

    .. raw:: html

       <title>

    Simple Bookstore App

    .. raw:: html

       </title>



7.  Paste the output of the following command in your browser address
    bar:

    .. code:: sh

      $ echo
    http://\ :math:`MY_INGRESS_GATEWAY_HOST:`\ INGRESS_PORT/productpage


8.  Simulate real-world user traffic to your application by setting an
    infinite loop in a new terminal window:

    .. code:: sh

      $ while :; do curl -s

    .. raw:: html

       <output of the previous command>

    \| grep -o "

    .. raw:: html

       <title>

    .\*

    .. raw:: html

       </title>

    "; sleep 1; done

    .. raw:: html

       <title>

    Simple Bookstore App

    .. raw:: html

       </title>

    .. raw:: html

       <title>

    Simple Bookstore App

    .. raw:: html

       </title>

    .. raw:: html

       <title>

    Simple Bookstore App

    .. raw:: html

       </title>

    .. raw:: html

       <title>

    Simple Bookstore App

    .. raw:: html

       </title>

    …

9.  Check the graph of your namespace in the Kiali console
    ``my-kiali.io/kiali/console``. (The ``my-kiali.io`` URL should be in
    your ``/etc/hosts`` file that you set
    `previously </docs/examples/microservices-istio/bookinfo-kubernetes/#update-your-etc-hosts-configuration-file>`_).

    This time, you can see that traffic arrives from two sources,
    ``unknown`` (the Kubernetes Ingress) and from
    ``istio-ingressgateway istio-system`` (the Istio Ingress Gateway).

.. image::kiali-ingress-gateway.png
   :alt:
   :caption:Kiali Graph Tab with Istio Ingress Gateway
   :width: 80%

10. At this point you can stop sending requests through the Kubernetes
    Ingress and use Istio Ingress Gateway only. Stop the infinite loop
    (``Ctrl-C`` in the terminal window) you set in the previous steps.
    In a real production environment, you would update the DNS entry of
    your application to contain the IP of Istio ingress gateway or
    configure your external Load Balancer.

11. Delete the Kubernetes Ingress resource:

    .. code:: sh

      $ kubectl delete ingress bookinfo
    ingress.extensions “bookinfo” deleted

12. In a new terminal window, restart the real-world user traffic
    simulation as described in the previous steps.

13. Check your graph in the Kiali console. After about a minute, you
    will see the Istio Ingress Gateway as a single source of traffic for
    your application.

.. image::kiali-ingress-gateway-only.png
   :alt:
   :caption:Kiali Graph Tab with Istio Ingress Gateway as a single source of traffic
   :width: 80%

You are ready to configure `logging with
Istio </docs/examples/microservices-istio/logs-istio>`_.
