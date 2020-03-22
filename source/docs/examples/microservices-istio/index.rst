Learn Microservices using Kubernetes and Istio
======================================================

This modular tutorial provides new users with hands-on experience using Istio for common microservices scenarios, one step at a time.

Prerequisites
Setup a Kubernetes Cluster
Setup a Local Computer
Run a Microservice Locally
Run ratings in Docker
Run Bookinfo with Kubernetes
Test in production
Add a new version of reviews
Enable Istio on productpage
Enable Istio on all the microservices
Configure Istio Ingress Gateway
Monitoring with Istio
It is intended for self-guided users or instructors who train others. It begins with the steps to set up a cluster to control an example microservice running on a local computer, and culminates into demonstrating several crucial microservice management tasks using Istio.

For the best experience, follow the modules in the order provided.

This is work in progress. We will add its sections in pieces. Your feedback is welcome at discuss.istio.io.

It is intended for self-guided users or instructors who train others. It
begins with the steps to set up a cluster to control an example
microservice running on a local computer, and culminates into
demonstrating several crucial microservice management tasks using Istio.

For the best experience, follow the modules in the order provided.

{{< boilerplate work-in-progress >}}

.. toctree::
   :maxdepth: 2

   add-istio/index
   add-mtls/index
   add-new-microservice-version/index
   bookinfo-kubernetes/index
   enable-istio-all-microservices/index
   istio-ingress-gateway/index
   logs-istio/index
   package-service/index
   prereq/index
   production-testing/index
   setup-kubernetes-cluster/index
   setup-local-computer/index
   single/index
