production-testing
============================================

{{< boilerplate work-in-progress >}}

Test your microservice, in production!

Testing individual microservices
--------------------------------

1. Issue an HTTP request from the testing pod to one of your services:

   .. code:: sh

      $ kubectl exec -it $(kubectl get pod -l app=sleep
   -o jsonpath=‘{.items[0].metadata.name}’) – curl
   http://ratings:9080/ratings/7

Chaos testing
-------------

Perform some `chaos
testing <http://www.boyter.org/2016/07/chaos-testing-engineering/>`_ in
production and see how your application reacts. After each chaos
operation, access the application’s webpage and see if anything changed.
Check the pods’ status with ``kubectl get pods``.

1. Terminate the ``details`` service in one pod.

   .. code:: sh

      $ kubectl exec -it $(kubectl get pods -l
   app=details -o jsonpath=‘{.items[0].metadata.name}’) – pkill ruby

2. Check the pods status:

   .. code:: sh

      $ kubectl get pods NAME READY STATUS RESTARTS AGE
   details-v1-6d86fd9949-fr59p 1/1 Running 1 47m
   details-v1-6d86fd9949-mksv7 1/1 Running 0 47m
   details-v1-6d86fd9949-q8rrf 1/1 Running 0 48m
   productpage-v1-c9965499-hwhcn 1/1 Running 0 47m
   productpage-v1-c9965499-nccwq 1/1 Running 0 47m
   productpage-v1-c9965499-tjdjx 1/1 Running 0 48m
   ratings-v1-7bf577cb77-cbdsg 1/1 Running 0 47m
   ratings-v1-7bf577cb77-cz6jm 1/1 Running 0 47m
   ratings-v1-7bf577cb77-pq9kg 1/1 Running 0 48m
   reviews-v1-77c65dc5c6-5wt8g 1/1 Running 0 47m
   reviews-v1-77c65dc5c6-kjvxs 1/1 Running 0 48m
   reviews-v1-77c65dc5c6-r55tl 1/1 Running 0 47m sleep-88ddbcfdd-l9zq4
   1/1 Running 0 47m

   Note that the first pod was restarted once.

3. Terminate the ``details`` service in all its pods:

   .. code:: sh

      $ for pod in $(kubectl get pods -l app=details -o
   jsonpath=‘{.items[*].metadata.name}’); do echo terminating $pod;
   kubectl exec -it $pod – pkill ruby; done

4. Check the webpage of the application:

   .. image::bookinfo-details-unavailable.png
      :alt:
      :caption:Bookinfo Web Application, details unavailable
      :width: 80%

   Note that the details section contains error messages instead of book
   details.

5. Check the pods status:

   .. code:: sh

      $ kubectl get pods NAME READY STATUS RESTARTS AGE
   details-v1-6d86fd9949-fr59p 1/1 Running 2 48m
   details-v1-6d86fd9949-mksv7 1/1 Running 1 48m
   details-v1-6d86fd9949-q8rrf 1/1 Running 1 49m
   productpage-v1-c9965499-hwhcn 1/1 Running 0 48m
   productpage-v1-c9965499-nccwq 1/1 Running 0 48m
   productpage-v1-c9965499-tjdjx 1/1 Running 0 48m
   ratings-v1-7bf577cb77-cbdsg 1/1 Running 0 48m
   ratings-v1-7bf577cb77-cz6jm 1/1 Running 0 48m
   ratings-v1-7bf577cb77-pq9kg 1/1 Running 0 49m
   reviews-v1-77c65dc5c6-5wt8g 1/1 Running 0 48m
   reviews-v1-77c65dc5c6-kjvxs 1/1 Running 0 49m
   reviews-v1-77c65dc5c6-r55tl 1/1 Running 0 48m sleep-88ddbcfdd-l9zq4
   1/1 Running 0 48m

   The first pod restarted twice and two other ``details`` pods
   restarted once. You may experience the ``Error`` and the
   ``CrashLoopBackOff`` statuses until the pods reach ``Running``
   status.

In both cases, the application did not crash. The crash in the
``details`` microservice did not cause other microservices to fail. This
behavior means you did not have a **cascading failure** in this
situation. Instead, you had **gradual service degradation**: despite one
microservice crashing, the application could still provide useful
functionality. It displayed the reviews and the basic information about
the book.

You are ready to `add a new version of the reviews
application </docs/examples/microservices-istio/add-new-microservice-version>`_.
