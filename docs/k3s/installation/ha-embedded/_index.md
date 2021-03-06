---
title: "嵌入式DB的高可用（实验）"
description: 从v1.0.0开始，K3s预览版支持运行高可用的control-plane，无需外部数据库。这意味着不需要管理外部etcd或SQL数据存储即可运行可靠的生产级设置。
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
  - 安装介绍
  - 嵌入式DB的高可用
---

## 概述

K3s 正在预览支持运行一个高可用的 control-plane，而不需要外部数据库。这意味着不需要管理外部的 etcd 或 SQL 数据存储。

在 K3s 1.0.0 中，使用 Dqlite 作为实验性的嵌入式数据库。在 K3s v1.19.1+中，使用的是嵌入式 etcd。

请注意，不支持从实验性的 Dqlite 升级到实验性的嵌入式 etcd。如果你尝试升级，将不会成功，数据将丢失。

## 嵌入式 etcd (实验性)

_从 K3s v1.19.1 开始可用_

要在这种模式下运行 K3s，你必须有奇数的服务器节点。我们建议从三个节点开始。

要开始运行，首先启动一个服务器节点，使用`cluster-init`标志来启用集群，并使用一个标记作为共享的秘密来加入其他服务器到集群中。

```
K3S_TOKEN=SECRET k3s server --cluster-init
```

启动第一台服务器后，使用共享秘密将第二台和第三台服务器加入集群。

```
K3S_TOKEN=SECRET k3s server --server https://<ip or hostname of server1>:6443。
```

现在你有了一个高可用的 control-plane。将额外的工作节点加入到集群中，步骤与单个服务器集群相同。

## 嵌入式 Dqlite(已废弃)

> **警告：**在 K3s v1.19.1 版本中，实验性的 etcd 取代了实验性的 Dqlite。这是一个突破性的变化。请注意，不支持从实验性 Dqlite 升级到实验性嵌入式 etcd。如果你尝试升级，它将不会成功，数据将丢失。
> 从 v1.0.0 开始，K3s 预览了对运行高可用 control-plane 的支持，而不需要外部数据库。

这种架构是通过在 K3s 服务器进程中嵌入一个 Dqlite 数据库来实现的。DQLite 是“嵌入式 SQLite”的简称，它是一个快速的、嵌入式的、具有 Raft 共识的持久性 SQL 数据库，非常适合容错的物联网和边缘设备。

要使用嵌入式 Dqlite 数据库运行 K3s，请按照上文[嵌入式 etcd 数据库](#embedded-etcd-experimental)相同的步骤，使用 v1.0.0 和 v1.19.1 之间的 K3s 版本。
