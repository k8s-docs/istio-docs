use-k8s-secrets
=====================

Yes. The key and certificate distribution in Istio Authentication is
based on `Kubernetes
secrets <https://kubernetes.io/docs/concepts/configuration/secret/>`_.

Secrets have known `security
risks <https://kubernetes.io/docs/concepts/configuration/secret/#risks>`_.
The Kubernetes team is working on `several
features <https://docs.google.com/document/d/1T2y-9geg9EfHHtCDYTXptCa-F4kQ0RyiH-c_M1SyD0s>`_
to improve Kubernetes secret security, from secret encryption to
node-level access control. And as of version 1.6, Kubernetes introduces
`RBAC
authorization <https://kubernetes.io/docs/reference/access-authn-authz/rbac/>`_,
which can provide fine-grained secrets management.
