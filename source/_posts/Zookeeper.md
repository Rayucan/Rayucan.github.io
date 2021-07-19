---
title: Zookeeper
date: 2021-02-02 20:18:39
tags: Web
categories: 笔记
description: 一个开源的分布式的，为分布式应用提供协调服务的Apache项目。
---

## Zookeeper简介

**分布、开源的应用程序协调服务**,它是集群的管理者,监视着集群中各个节点的状态，根据节点的反馈进行下一步合理操作。主要解决分布式应用经常遇到的数据管理问题,如：统一命名服务、状态同步服务、集群管理、分布式应用配置项的管理等。



### 工作机制

Zookeeper从设计模式角度来理解：

是一个基于观察者模式设计的分布式服务管理框架，它负责存储和管理大家都关心的数据，然后接受观察者的注册， 一旦这些数据的状态发生变化，Zookeeper就将负责通知已经在Zookeeper上注册的那些观察者做出相应的反应。

### 特点

![image-20210202203302748](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202203302.png)

1. Zookeeper：一个领导者（Leader），多个跟随者（Follower）组成的集群。
2. 集群中只要有半数以上节点存活，Zookeeper集群就能正常服务。
3. 全局数据一致：每个Server保存一份相同的数据副本，Client无论连接到哪个Server，数据都是一致的。
4. 更新请求顺序进行，来自同一个Client的更新请求按其发送顺序依次执行。
5. 数据更新原子性，一次数据更新要么成功，要么失败。
6. 实时性，在一定时间范围内，Client能读到最新数据。

### 数据结构

Zookeeper数据模型的结构与Unix文件系统很类似，整体上可以看作是一棵树，每个节点称做一个ZNode。

每一个ZNode默认能够存储1MB的数据，每个ZNode都可以通过其路径唯一标识。

![image-20210202203552571](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202203552.png)

### 应用场景

提供的服务包括：统一命名服务、统一配置管理、统一集群管理、服务器节点动态上下线、软负载均衡等

#### 统一命名服务

在分布式环境下，经常需要对应用/服务进行统一命名，便于识别。
例如：IP不容易记住，而域名容易记住。

![1](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202204715.jpg)

#### 统一配置管理

1. 分布式环境下，配置文件同步非常常见。

   （1）一般要求一个集群中，所有节点的配置信息是一致的，比如Kafka 集群。

   （2）对配置文件修改后，希望能够快速同步到各个节点上。

2. 配置管理可交由ZooKeeper实现。

   （1）可将配置信息写入ZooKeeper上的一个Znode。

   （2）各个客户端服务器监听这个Znode。

   （3）一旦Znode中的数据被修改，ZooKeeper将通知各个客户端服务器。

####  统一集群管理

1. 分布式环境中，实时掌握每个节点的状态是必要的。
   （1）可根据节点实时状态做出一些调整。
2. ZooKeeper可以实现实时监控节点状态变化
   （1）可将节点信息写入ZooKeeper上的一个ZNode。
   （2）监听这个ZNode可获取它的实时状态变化。

#### 服务器动态上下线

![image-20210202205120751](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202205120.png)

#### 软负载均衡

在Zookeeper中记录每台服务器的访问数，让访问数最少的服务器去处理最新的客户端请求

![image-20210202205147777](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202205147.png)

## Zookeeper内部原理

### 节点类型

持久（Persistent）：客户端和服务器端断开连接后，创建的节点不删除

短暂（Ephemeral）：客户端和服务器端断开连接后，创建的节点自己删除



说明：创建znode时设置顺序标识，znode名称后会附加一个值，顺序号是一个单调递增的计数器，由父节点维护 

注意：在分布式系统中，顺序号可以被用于为所有的事件进行全局排序，这样客户端可以通过顺序号推断事件的顺序



1. 持久化目录节点

   客户端与Zookeeper断开连接后，该节点依旧存在

2. 持久化顺序编号目录节点

   客户端与Zookeeper断开连接后，该节点依旧存在，只是Zookeeper给该节点名称进行顺序编号

3. 临时目录节点

   客户端与Zookeeper断开连接后，该节点被删除

4. 临时顺序编号目录节点

   客户端 与 Zookeeper 断开连接后 ， 该 节 点 被 删 除 ， 只 是 Zookeeper给该节点名称进行顺序编号。

### Stat结构体

1. czxid-创建节点的事务 zxid

   每次修改 ZooKeeper 状态都会收到一个 zxid 形式的时间戳，也就是 ZooKeeper 事务ID。 事务ID是 ZooKeeper 中所有修改总的次序。每个修改都有唯一的 zxid，如果 zxid1 小于 zxid2，那么 zxid1 在 zxid2 之前发生。

2. ctime - znode 被创建的毫秒数(从 1970 年开始)

3. mzxid - znode 最后更新的事务 zxid

4. mtime - znode 最后修改的毫秒数(从 1970 年开始)

5. pZxid-znode 最后更新的子节点 zxid

6. cversion - znode 子节点变化号，znode 子节点修改次数

7. dataversion - znode 数据变化号

8. aclVersion - znode 访问控制列表的变化号

9. ephemeralOwner- 如果是临时节点，这个是 znode 拥有者的 session id。如果不是临时节 点则是 0

10. dataLength- znode 的数据长度

11. numChildren - znode 子节点数量

### 监听器原理（重点）

* 监听原理

  1. 首先要有一个main()线程
  2. 在main线程中创建Zookeeper客户端，这时就会创建两个线程，一个负责网络连接通信（connet），一个负责监听（listener）
  3. 通过connect线程将注册的监听事件发送给Zookeeper
  4. 在Zookeeper的注册监听器列表中将注册的监听事件添加到列表中
  5. Zookeeper监听到有数据或路径变化，就会将这个消息发送给listener线程
  6. listener线程内部调用了process()方法

* 常见的监听

  1. 监听节点数据的变化

     ```
     get path [watch]
     ```

  2. 监听子节点增减的变化

     ```
     ls path [watch]
     ```

![image-20210202211000680](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202211000.png)

### 选举机制（重点）

1. 半数机制

   集群中半数以上机器存活，集群可用。所以 Zookeeper 适合安装奇数台服务器。

2. Zookeeper 虽然在配置文件中并没有指定 Master 和 Slave，但是，Zookeeper 工作时， 是有一个节点为 Leader，其他则为 Follower，Leader 是通过内部的选举机制临时产生的。



**以一个简单的例子来说明整个选举的过程**

假设有五台服务器组成的 Zookeeper 集群，它们的 id 从1-5，同时它们都是最新启动的， 也就是没有历史数据，在存放数据量这一点上，都是一样的。假设这些服务器依序启动，来看看会发生什么

![image-20210202211233621](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202211233.png)

1. 服务器 1 启动，发起一次选举。服务器 1 投自己一票。此时服务器 1 票数一票， 不够半数以上（3 票），选举无法完成，服务器 1 状态保持为 LOOKING； 
2. 服务器 2 启动，再发起一次选举。服务器 1 和 2 分别投自己一票并交换选票信息： 此时服务器 1 发现服务器 2 的 ID 比自己目前投票推举的（服务器 1）大，更改选票为推举 服务器 2。此时服务器 1 票数 0 票，服务器 2 票数 2 票，没有半数以上结果，选举无法完成， 服务器 1，2 状态保持 LOOKING 
3. 服务器 3 启动，发起一次选举。此时服务器 1 和 2 都会更改选票为服务器 3。此 次投票结果：服务器 1 为 0 票，服务器 2 为 0 票，服务器 3 为 3 票。此时服务器 3 的票数已 经超过半数，服务器 3 当选 Leader。服务器 1，2 更改状态为 FOLLOWING，服务器 3 更改 状态为 LEADING； 
4. 服务器 4 启动，发起一次选举。此时服务器 1，2，3 已经不是 LOOKING 状态， 不会更改选票信息。交换选票信息结果：服务器 3 为 3 票，服务器 4 为 1 票。此时服务器 4 服从多数，更改选票信息为服务器 3，并更改状态为 FOLLOWING； 
5. 服务器 5 启动，同 4 一样当小弟。

### 数据流程

![image-20210202211342050](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210202211342.png)