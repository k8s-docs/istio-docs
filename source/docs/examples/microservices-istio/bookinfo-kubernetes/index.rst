bookinfo-kubernetes
============================================

{{< boilerplate work-in-progress >}}

This module shows you an application composed of four microservices
written in different programming languages: ``productpage``,
``details``, ``ratings`` and ``reviews``. We call the composed
application ``Bookinfo``, and you can learn more about it on the
`Bookinfo example </docs/examples/bookinfo>`_ page.

The `Bookinfo example </docs/examples/bookinfo>`_ shows the final state
of the application, in which the ``reviews`` microservice has three
versions: ``v1``, ``v2``, ``v3``. In this module, the application only
uses the ``v1`` version of the ``reviews`` microservice. The next
modules enhance the application by deploying newer versions of the
``reviews`` microservice.

Deploy the application and a testing pod
----------------------------------------

1. Set the ``MYHOST`` environment variable to hold the URL of the
   application:

   .. code:: sh

      $ export MYHOST=$(kubectl config view -o
   jsonpath={.contexts..namespace}).bookinfo.com

2. Skim
   `bookinfo.yaml <%7B%7B%3C%20github_blob%20%3E%7D%7D/samples/bookinfo/platform/kube/bookinfo.yaml>`_.
   This is the Kubernetes deployment spec of the app. Notice the
   services and the deployments.

3. Deploy the application to your Kubernetes cluster:

   .. code:: sh

      $ kubectl apply -l version!=v2,version!=v3 -f {{<
   github_file >}}/samples/bookinfo/platform/kube/bookinfo.yaml service
   “details” created deployment “details-v1” created service “ratings”
   created deployment “ratings-v1” created service “reviews” created
   deployment “reviews-v1” created service “productpage” created
   deployment “productpage-v1” created

4. Check the status of the pods:

   .. code:: sh

      $ kubectl get pods NAME READY STATUS RESTARTS AGE
   details-v1-6d86fd9949-q8rrf 1/1 Running 0 10s
   productpage-v1-c9965499-tjdjx 1/1 Running 0 8s
   ratings-v1-7bf577cb77-pq9kg 1/1 Running 0 9s
   reviews-v1-77c65dc5c6-kjvxs 1/1 Running 0 9s

5. After the four services achieve the ``Running`` status, you can scale
   the deployment. To let each version of each microservice run in three
   pods, execute the following command:

   .. code:: sh

      $ kubectl scale deployments –all –replicas 3
   deployment “details-v1” scaled deployment “productpage-v1” scaled
   deployment “ratings-v1” scaled deployment “reviews-v1” scaled
   deployment “reviews-v2” scaled deployment “reviews-v3” scaled

6. Check the pods status. Notice that each microservice has three pods:

   .. code:: sh

      $ kubectl get pods NAME READY STATUS RESTARTS AGE
   details-v1-6d86fd9949-fr59p 1/1 Running 0 50s
   details-v1-6d86fd9949-mksv7 1/1 Running 0 50s
   details-v1-6d86fd9949-q8rrf 1/1 Running 0 1m
   productpage-v1-c9965499-hwhcn 1/1 Running 0 50s
   productpage-v1-c9965499-nccwq 1/1 Running 0 50s
   productpage-v1-c9965499-tjdjx 1/1 Running 0 1m
   ratings-v1-7bf577cb77-cbdsg 1/1 Running 0 50s
   ratings-v1-7bf577cb77-cz6jm 1/1 Running 0 50s
   ratings-v1-7bf577cb77-pq9kg 1/1 Running 0 1m
   reviews-v1-77c65dc5c6-5wt8g 1/1 Running 0 49s
   reviews-v1-77c65dc5c6-kjvxs 1/1 Running 0 1m
   reviews-v1-77c65dc5c6-r55tl 1/1 Running 0 49s

7. After the services achieve the ``Running`` status, deploy a testing
   pod, `sleep <%7B%7B%3C%20github_tree%20%3E%7D%7D/samples/sleep>`_,
   to use for sending requests to your microservices:

   .. code:: sh

      $ kubectl apply -f {{< github_file
   >}}/samples/sleep/sleep.yaml

8. To confirm that the Bookinfo application is running, send a request
   to it with a curl command from your testing pod:

   .. code:: sh

      $ kubectl exec -it $(kubectl get pod -l app=sleep
   -o jsonpath=‘{.items[0].metadata.name}’) -c sleep – curl
   productpage:9080/productpage \| grep -o "

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



Enable external access to the application
-----------------------------------------

Once your application is running, enable clients from outside the
cluster to access it. Once you configure the steps below successfully,
you can access the application from your laptop’s browser.

.. warning::



If your cluster runs on GKE, change the ``productpage`` service type to
``LoadBalancer``:

.. code:: sh

      $ kubectl patch svc productpage -p ‘{“spec”: {“type”:
“LoadBalancer”}}’ service/productpage patched



Configure the Kubernetes Ingress resource and access your application’s webpage
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Create a Kubernetes Ingress resource:

   .. code:: sh

      $ kubectl apply -f - <<EOF apiVersion:
   extensions/v1beta1 kind: Ingress metadata: name: bookinfo spec:
   rules:

   -  host: $MYHOST http: paths:

      -  path: /productpage backend: serviceName: productpage
         servicePort: 9080
      -  path: /login backend: serviceName: productpage servicePort:
         9080
      -  path: /logout backend: serviceName: productpage servicePort:
         9080
      -  path: /static/\* backend: serviceName: productpage servicePort:
         9080 EOF

Update your ``/etc/hosts`` configuration file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Get the IP address for the Kubernetes ingress named ``bookinfo``:

   .. code:: sh

      $ kubectl get ingress bookinfo

2. In your ``/etc/hosts`` file, add the previous IP address to the host
   entries provided by the following command. You should have a
   `Superuser <https://en.wikipedia.org/wiki/Superuser>`_ privilege and
   probably use `sudo <https://en.wikipedia.org/wiki/Sudo>`_ to
   edit ``/etc/hosts``.

   .. code:: sh

      $ echo $(kubectl get ingress istio-system -n
   istio-system -o jsonpath=‘{..ip} {..host}’) $(kubectl get ingress
   bookinfo -o jsonpath=‘{..host}’)

Access your application
~~~~~~~~~~~~~~~~~~~~~~~

1. Access the application’s home page from the command line:

   .. code:: sh

      $ curl -s $MYHOST/productpage \| grep -o "

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



2. Paste the output of the following command in your browser address
   bar:

   .. code:: sh

      $ echo http://$MYHOST/productpage

   You should see the following webpage:

   .. image::bookinfo.png
      :alt:
      :caption:Bookinfo Web Application
      :width: 80%

3. Observe how microservices call each other. For example, ``reviews``
   calls the ``ratings`` microservice using the
   ``http://ratings:9080/ratings`` URL. See the `code of
   ``reviews`` <%7B%7B%3C%20github_blob%20%3E%7D%7D/samples/bookinfo/src/reviews/reviews-application/src/main/java/application/rest/LibertyRestEndpoint.java>`_:

   {{< text java >}} private final static String ratings_service =
   “http://ratings:9080/ratings”;

4. Set an infinite loop in a separate terminal window to send traffic to
   your application to simulate the constant user traffic in the real
   world:

   .. code:: sh

      $ while :; do curl -s $MYHOST/productpage \| grep
   -o "

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

You are ready to `test the
application </docs/examples/microservices-istio/production-testing>`_.
