---
title: 跨区域多集群部署
weight: 73
---

在阿里云容器服务中部署了多个 Kubernetes 集群，每个集群位于不同的区域中。 如果最靠近用户的实例发生故障或过载，则流量将无缝地定向到另一个可用实例。基于 Istio，可以轻松地跨多个区域中的 Kubernetes 集群实现流量统一管理。

前提条件
您已经成功创建一个 Kubernetes 集群，参见创建 Kubernetes 集群。
请以主账号登录，或赋予子账号足够的权限，如自定义角色中的 cluster-admin，参考子账号 RBAC 权限配置指导。
基于 Flat 网络或者 VPN 的多集群部署方式，多个 Kubernetes 集群应位于同一个 VPC 下网络互通，或者通过云企业网、高速通道等打通跨 VPC 网络的访问；同时要求 Pod/Service CIDR 不能冲突。
以下文档使用基于 Flat 网络或者 VPN 的多集群 Istio 部署方式管理服务。
背景信息
容器服务 Kubernetes 版（简称 ACK）提供高性能可伸缩的容器应用管理能力，支持企业级 Kubernetes 容器化应用的全生命周期管理。

阿里云容器服务 Kubernetes 1.10.4 及之后版本支持部署 Istio，如果是 1.10.4 之前的版本，请先升级到 1.10.4 或之后版本。
一个集群中，worker 节点数量需要大于等于 3 个，保证资源充足可用。
配置多集群网络
同一 VPC 下
同一 VPC 下，无论集群是在同一交换机下还是多个交换机下，网络互通，多个安全组需要手工设置打通。不同安全组默认情况下不能互相访问，同一 VPC 下可以通过设置规则使其互通，请参见添加安全组规则。添加安全组
不同 VPC 下
不同 VPC 下，无论是否在相同地域、可用区，集群之间网络本身不能互通，需要设置 VPC 互通（云企业网、VPN 网关、高速通道），云企业网的配置更加简单而且可以自动分发学习路由，因此，推荐您使用云企业网。
说明 VPC 网络不能冲突，默认建立集群时创建的 VPC 网络段都是 192.168.0.0/16；所以需要手工创建 VPC，并指定不同的网段。
此外，多个安全组需要手工设置打通，可以参考添加安全组规则。

数据平面上的安全组添加对应的控制平面的网段：添加控制平面网段
类似地，控制平面上的安全组也同时添加相应的网段。

此外，网络规划需要提前设置好，保证 2 个集群内的 service/pod CIDR、VPC CIDR 都不能重叠。
通过集群界面部署 Istio
登录容器服务管理控制台。
在 Kubernetes 菜单下，单击左侧导航栏的集群 > 集群，进入集群列表页面。
选择所需的集群并单击操作列更多 > 部署 Istio。
部署 istio
针对多集群部署，根据如下信息进行配置：
与部署 Istio 到单个集群的方式一样，可以使用阿里云提供的控制台一键部署，请参见部署 Istio。
配置 说明
启用服务就近访问 在跨区域的多集群部署中是否启用服务就近访问。默认情况下不启用。
多集群同一控制平面模式设置
不启用：不启用多集群模式。
使用 Flat 网络或者 VPN：基于 Flat 网络或者 VPN 方式实现多集群中的网络，多集群间的 Pod 可以互通。
不使用 Flat 网络或者 VPN：不使用 Flat 网络或者 VPN，多集群之间仅通过网关进行交互。
单击部署 Istio，启动部署。
在部署页面下方，可实时查看部署进展及状态。查看部署状态
预期结果

单击左侧导航栏应用 > 容器组，选择部署 Istio 的集群及命名空间，可查看到已经部署 Istio 的相关容器组。查看容器组
管理 Istio Ingress Gateway
上述部署 Istio 之后，包含一个入口网关 Ingress Gateway，该网关使用公网 IP 的负载均衡。通过该网关将集群中应用服务暴露到集群之外。

执行 kubectl get service -n istio-system -l istio=ingressgateway，获取入口网关的公网 IP 地址，应用访问可以通过该 Gateway 进行。管理 Istio 网关
将数据平面加入 Istio 管理
在数据平面的 Kubernetes 集群上执行如下生成 kubeconfig 文件。
kubectl create namespace istio-system
kubectl apply -f http://istio.oss-cn-hangzhou.aliyuncs.com/istio-operator/rbac.yml
wget http://istio.oss-cn-hangzhou.aliyuncs.com/istio-operator/generate-kubeconfig.sh
chmod +x generate-kubeconfig.sh
./generate-kubeconfig.sh ${CLUSTER_NAME}
说明 ${CLUSTER_NAME}需要在整个服务网格内保证唯一。
在 Istio 控制平面集群下执行如下命令。
wget http://istio.oss-cn-hangzhou.aliyuncs.com/istio-operator/create-secret.sh
chmod +x create-secret.sh
./create-secret.sh ${CLUSTER_NAME}
在Istio控制平面集群上，创建并拷贝内容到${CLUSTER_NAME}.yaml 文件中，并执行 kubectl apply -n istio-system -f ${CLUSTER_NAME}.yaml。
apiVersion: istio.alibabacloud.com/v1beta1
kind: RemoteIstio
metadata:
  name: ${CLUSTER_NAME}
namespace: istio-system
spec:
autoInjectionNamespaces:

- default
  dockerImage:
  region: cn-beijing
  gateways:
  k8singress: {}
  hub: registry.cn-beijing.aliyuncs.com/aliacs-app-catalog
  includeIPRanges: '\*'
  proxy: {}
  security: {}
  sidecarInjector:
  enabled: true
  replicaCount: 1
  预期结果：

可通过以下操作查看远程集群的 Istio 部署是否成功：
切换到远程集群，单击左侧导航栏应用 > 容器组，进入容器组页面。
选择部署 Istio 的集群及命名空间，可查看到已经部署 Istio 的相关容器组。远程查看容器组
