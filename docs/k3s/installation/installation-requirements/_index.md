---
title: 安装要求
description: K3s 非常轻巧，但有一些最低要求，如下所述。无论您是将 K3s 集群配置为在 Docker 还是 Kubernetes 设置中运行，运行 K3s 的每个节点都应该满足以下最低要求。你可能需要更多的资源来满足你的需求。
keywords:
  - k3s中文文档
  - k3s 中文文档
  - k3s中文
  - k3s 中文
  - k3s
  - k3s教程
  - k3s中国
  - rancher
  - k3s 中文教程
  - 安装要求
---

K3s 非常轻巧，但有一些最低要求，如下所述。

无论您是将 K3s 集群配置为在 Docker 还是 Kubernetes 设置中运行，运行 K3s 的每个节点都应该满足以下最低要求。你可能需要更多的资源来满足你的需求。

## 先决条件

- 两个节点不能有相同的主机名。

- 如果您的所有节点都有相同的主机名，请使用`--with-node-id`选项为每个节点添加一个随机后缀，或者为您添加到集群的每个节点设计一个独特的名称，用`--node-name`或`$K3S_NODE_NAME`传递。

## 操作系统

K3s 应该可以运行在几乎所有类型的 Linux 主机上。

K3s 在以下操作系统及其后续非主要版本中得到官方支持和测试：

- Ubuntu 16.04 (amd64)
- Ubuntu 18.04 (amd64)
- Raspbian Buster\*

\* 如果您使用的是 **Raspbian Buster**, 请按照[这些步骤](/docs/k3s/advanced/_index#在raspbian-buster上启用旧版的iptables)切换到传统的 iptables。

如果您使用的是**Alpine Linux**，请按照[这些步骤](/docs/k3s/advanced/_index#alpine-linux安装的额外准备工作)进行额外的设置。

## 硬件

硬件要求根据您部署的规模而变化。这里列出了最低建议。

- 内存： 最低 512MB（建议至少为 1GB）
- CPU： 最低 1

### 磁盘

K3s 的性能取决于数据库的性能。为了确保最佳速度，我们建议尽可能使用 SSD。在使用 SD 卡或 eMMC 的 ARM 设备上，磁盘性能会有所不同。

## 网络

K3s server 需要 6443 端口才能被节点访问。

当使用 Flannel VXLAN 时，节点需要能够通过 UDP 端口 8472 访问其他节点。节点不应该在其他端口上监听。K3s 使用反向隧道，这样节点与服务器建立出站连接，所有的 kubelet 流量都通过该隧道运行。然而，如果你不使用 Flannel 并提供自己的自定义 CNI，那么 K3s 就不需要 8472 端口。

如果要使用`metrics server`，则需要在每个节点上打开端口 10250 端口。

> **重要：** 节点上的 VXLAN 端口不应暴露给全世界，因为它公开了集群网络，任何人都可以访问它。应在禁止访问端口 8472 的防火墙/安全组后面运行节点。

<figcaption>K3s Server节点的入站规则：</figcaption>

| 协议 | 端口  | 源                       | 描述                      |
| :--- | :---- | :----------------------- | :------------------------ |
| TCP  | 6443  | K3s agent 节点           | Kubernetes API            |
| UDP  | 8472  | K3s server 和 agent 节点 | R 仅对 Flannel VXLAN 需要 |
| TCP  | 10250 | K3s server 和 agent 节点 | kubelet                   |

通常情况下，所有出站流量都是允许的。

## 大型集群

硬件要求取决于您的 K3s 集群的大小。对于生产和大型集群，我们建议使用具有外部数据库的高可用性设置。对于生产中的外部数据库，建议使用以下选项：

- MySQL
- PostgreSQL
- etcd

### CPU 和 内存

以下是高可用 K3s server 中节点的最低 CPU 和内存要求：

| 部署规模 |   节点    | VCPUS |  RAM  |
| :------: | :-------: | :---: | :---: |
|  Small   | Up to 10  |   2   | 4 GB  |
|  Medium  | Up to 100 |   4   | 8 GB  |
|  Large   | Up to 250 |   8   | 16 GB |
| X-Large  | Up to 500 |  16   | 32 GB |
| XX-Large |   500+    |  32   | 64 GB |

### 磁盘

集群性能取决于数据库性能。为了确保最佳速度，我们建议始终使用 SSD 磁盘来支持你的 K3s 集群。在云提供商上，您还需要使用允许最大 IOPS 的最小 size。

### 网络

你应该考虑增加集群 CIDR 的子网大小，以免 Pod 的 IP 耗尽。你可以通过在启动时向 K3s 服务器传递`--cluster-cidr`选项来实现。

### 数据库

K3s 支持不同的数据库，包括 MySQL、PostgreSQL、MariaDB 和 etcd，以下是运行大型集群所需的数据库资源的大小指南：

| 部署规模 |   节点    | VCPUS |  RAM  |
| :------: | :-------: | :---: | :---: |
|  Small   | Up to 10  |   1   | 2 GB  |
|  Medium  | Up to 100 |   2   | 8 GB  |
|  Large   | Up to 250 |   4   | 16 GB |
| X-Large  | Up to 500 |   8   | 32 GB |
| XX-Large |   500+    |  16   | 64 GB |
