---
title: Istio 常见问题
weight: 76
---

您可通过本文了解 Istio 的常见问题及解决方法。

集群内无法访问集群外的 URL
问题现象：

集群内无法访问集群外的 URL。

问题原因：

缺省情况下，Istio 服务网格内的 Pod，由于其 iptables 将所有外发流量都透明的转发给了 Sidecar，所以这些集群内的服务无法访问集群之外的 URL，而只能处理集群内部的目标。

解决方法：

通过定义 ServiceEntry 来调用外部服务。
配置 Istio 使其直接放行对特定 IP 地址范围的访问。
详细内容，可参考 Control Egress Traffic。

Tiller 版本较低
问题现象：

安装过程中遇到如下提示。
Can't install release with errors: rpc error: code = Unknown desc = Chart incompatible with Tiller v2.7.0
问题原因：

Tiller 版本较低，需要升级。

解决方法：

执行以下任意一条命令，升级 Tiller 版本。
说明 此处请升级到 v2.10.0 及以上版本。
升级到 v2.11.0 版本：
helm init --tiller-image registry.cn-hangzhou.aliyuncs.com/acs/tiller:v2.11.0 --upgrade
升级到 v2.10.0 版本：
helm init --tiller-image registry.cn-hangzhou.aliyuncs.com/acs/tiller:v2.10.0 --upgrade
说明 Tiller 版本升级后，建议将客户端也升级到相应版本，客户端下载地址，请参考https://github.com/helm/helm/releases。
CRD（Custom Resource Definitions）版本问题
问题现象：

在第一次创建或升级 Istio 1.0 时遇到如下提示。
Can't install release with errors: rpc error: code = Unknown desc = apiVersion "networking.istio.io/v1alpha3" in ack-istio/charts/pilot/templates/gateway.yaml is not available
问题原因：

CRD 不存在或版本较低，需要安装最新版本的 CRD。
说明 仅 Helm 版本为 2.10.0 及之前版本，会遇到此问题。2.10.0 之后的版本，系统自动升级 CRD。
解决方法：

下载新版本的 Istio，可参考 Downloading the Release。
执行以下命令，安装最新版本的 CRD。
kubectl apply -f install/kubernetes/helm/istio/templates/crds.yaml -n istio-system
如果启用了 certmanager，需要执行以下命令安装相关的 CRD。
kubectl apply -f install/kubernetes/helm/istio/charts/certmanager/templates/crds.yaml
子账号无法安装 Istio
问题现象：

安装过程中遇到类似如下提示。
Error from server (Forbidden): error when retrieving current configuration of:
Resource: "apiextensions.k8s.io/v1beta1, Resource=customresourcedefinitions", GroupVersionKind: "apiextensions.k8s.io/v1beta1, Kind=CustomResourceDefinition"
问题原因：

当前使用账号不具有安装 Istio 的权限。

解决方法：

切换为主账号登录。
对子账号赋予相应的权限，例如自定义角色中的 cluster-admin，可参考子账号 RBAC 权限配置指导。
卸载 Istio 后 CRD 残留
问题现象：

卸载 Istio 后，CRD 残留。

问题原因：

卸载 Istio，系统不会删除 CRD，需要执行命令删除。

解决方法：

Helm 为 2.9.0 及之前版本，需要执行以下命令，删除 Job 资源。
kubectl -n istio-system delete job --all
执行以下命令，删除 CRD。
kubectl delete crd `kubectl get crd | grep -E 'istio.io|certmanager.k8s.io' | awk '{print $1}'`
删除后 custom resource 对象残留
问题现象：

卸载 Istio 后，custom resource 对象残留。

问题原因：

卸载 Istio，用户先删除了 crd，但没有删除 custom resource 对象，需要执行命令删除。

解决方法：

执行 kubectl edit istio -n istio-system istio-config 命令。删除对象 1
删除 finalizers 下的 - istio-operator.finializer.alibabacloud.com。删除对象 2
