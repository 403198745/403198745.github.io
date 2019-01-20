---
layout: post
title: Cloud Foundry概述和组成
categories: Blog
description: 对Cloud Foundry内容组成进行了一个学习
keywords: CF, Cloud Foundry,Component
---
# CF概述

标签（空格分隔）： CF Cloud-Foundry CF组件

---

## Cloud Foundry Overview
### 如何平衡负载？
CF 通过许多虚拟机来处理负载，通过以下三步来完成

 1. [Bosh][1] 在物理机器上创建并且发布虚拟机，同时发布和运行CF在云（虚拟机）上
 2. CF Controller 对虚拟机云上的应用进行管理 
 3. 通过路由来管理对于应用的访问

### How Apps Run AnyWhere  

CF指定2张虚拟机——组件虚拟器用来当做平台的基础设施，主机虚拟机，用来发布应用，Diego在所有主机虚拟机托管应用程序
为了满足需求，许多虚拟机可以运行相同应用程序的重复实例。这意味着应用程序必须是可移植的。  
CF发布应用程序源代码以及所有虚拟机需要编译运行应用程序的东西到虚拟机上。This includes the OS stack that the app runs on, and a buildpack containing all languages, libraries, and services that the app uses.  
在推送一个APP到虚拟机上时，Before sending an app to a VM, the Cloud Controller stages it for delivery by combining stack, buildpack, and source code into a droplet that the VM can unpack, compile, and run. 

### CF如何组织用户和工作区  

通过[Orgs and Spaces][2]来进行用户角色管理，通过uAA进行用户管理。

### CF内部组件怎么通信

云计算组件间的通信通过在内部使用HTTP和HTTPS协议发布信息，或者通过直接相互发送NAT消息来通信。 

# CF Components

> CF包含了一个自我服务的应用程序执行引擎，一个可以自动管理应用程序发布和生命周期管理的引擎和一个脚本命令行CLI，同时继承了可以简化发布流程的发布工具。  
Cloud Foundry具有开放式架构，包括用于添加框架的buildpack，应用程序服务接口和云提供程序接口。

![构成图][3]
---

## Routing
使用[GoRouter][4]进行Routing管理，路由到相应的组件，即云控制器组件或在Diego Cell上运行的托管应用程序 Router定期查询Diego Bulletin Board System (BBS)公告板系统，来确定每个应用程序单钱运行的单元和容器  
使用该信息，路由器根据每个单元虚拟机（VM）的IP地址和单元的容器的主机端端口号来重新计算新的路由表。  

## APP生命周期  

### Cloud Controller and Diego Brain
云控制器CC负责应用程序的部署，要将应用推送到Cloud Foundry，您可以指定Cloud Controller， 云控制器然后指导Diego Brain通过CC-Bridge来协调各个Diego单元，以运行和运行应用程序。  
在Diego使用前前，Cloud Controller的Droplet执行代理（DEA）执行了这些应用程序生命周期任务。
云控制器还维护组织，空间，用户角色，服务等的记录。  
### nsync,BBS,and Cell Reps
为了使应用程序可用，云部署必须不断监视其状态，并将其与预期状态协调一致，根据需要启动和停止进程。 在Diego前，使用Health Manager（HM9000）实现此功能。 nsync，BBS和Cell Reps使用更分散的方法。
![运行图][5]
nsync，BBS和Cell Rep组件沿着一条链路，以保持应用程序的运行。 一方面是用户。 另一方面是在广泛分布的虚拟机上运行的应用程序实例，可能会崩溃或变得不可用。  
下面是组件一起工作的方式：

 - 当用户访问应用程序时，nsync会从云端控制器CC接收消息。 它将实例数量以DesiredLRP结构的型式存入Diego BBS数据库中。 
 - BBS使用其收敛过程来监测DesiredLRP 和 ActualLRP 的值。 它会适当启动或杀死应用程序实例，以确保ActualLRP计数符合所需的LRP计数。
 - Cell Rep监控容器并提供ActualLRP值。  
   
## App Storage and Execution
### BlobStore
blobstore是大型二进制文件的存储库，Github是为代码设计的,它为文件存储。 Blobstore二进制文件包括：  

 - Application code packages
 - BuildPacks
 - Droplets

### Diego Cell
每个应用程序虚拟机都有一个Diego Cell，可以在本地执行应用程序启动和停止操作，管理虚拟机容器，同时将应用程序状态和其他数据报告给BBS和Loggregator。  

## Services 
应用程序通常依赖于数据库或第三方SaaS提供商等服务。 当开发人员提供并将服务绑定到应用程序时，该服务的服务代理负责提供服务实例。

## Messaging
### Consul and BBS
Cloud Foundry组件虚拟机通过HTTP和HTTPS协议在内部进行通信，在两个位置共享临时消息和存储的数据：

 - Consul Server服务器存储较长寿命的控制数据，例如组件IP地址
 - Diego’s Bulletin Board System (BBS)存储更新频繁和一次性的数据，如单元和应用程序状态，未分配的工作。BBS使用Go MySQL驱动程序将数据存储在MySQL中。
  
路由发射器组件（ route-emitter component ）使用NATS协议向路由器广播最新的路由表。 在Diego架构前，NATS消息总线承载了所有内部组件通信。

 
  [1]: http://bosh.io
  [2]: https://docs.cloudfoundry.org/concepts/roles.html
  [3]: https://docs.cloudfoundry.org/concepts/images/cf_architecture_block.png
  [4]: https://docs.cloudfoundry.org/concepts/architecture/router.html
  [4]: https://docs.cloudfoundry.org/concepts/images/diego/app-monitor-sync-diego.png
