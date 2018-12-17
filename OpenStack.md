###   OpenStack 

###   一   概述

####   1 计算机操作系统原理

ＣPU 的三大核心控件：运算器，寄存器（暂存数据），控制器（计算-存储）  

CPU 寻址 cell --->  RAM  --- North Bridge  ---> 

64 bit 存储约 1T 数据 

PAE : 物理地址扩展 32bit + 4bit = 64G 多加4位寻址总线扩展，单进程使用内存最大 64 G

缓存提高速度： CPU --  RAM 程序具有局部性（空间局部，时间局部），热驱动数据保存在缓存中，20% 热驱动数据，置换策略

多级缓存，一级指令和数据缓存，二级缓存...  静态 RAM 特性（不停的刷新），造价高一级是二级的2倍  32K  64K

####   2 Linux 操作系统原理

操作系统：多进程，多任务

###  二   云计算中使用

####   1  OpenStack

Nova-compute ：守护进程，创建或终止一个虚拟机实例

架构：

Glance：注册并取得虚拟机相关映射信息，数据，云数据信息存储

Swift：分布式文件系统

Nova-network ：真正提供服务的，用户使用地址

Nova-volume ：提供，弥补数据的持久化存储



###  三   文档知识点

###   四  快速入门 OpenStack

1 虚拟化：一种资源管理技术，将实体的资源，如服务器，网络，内存存储等，抽象，转换后呈现出来，优点：提高硬件资源的利用率，部署多个虚拟机，提高服务器利用率

2 云计算：一种业务模式，按需分配，随时伸缩；

私有云，共有云，混合云 12306（云服务器，数据安全）阿里云提供查询服务

三种业务模式：基础设施器服务阿里云，腾讯，亚马逊等；中间件和数据库自己部署

阿帕奇，mysql虚拟主机， 阿帕斯服务，新浪

360，美团等萨斯服务，邮箱网易，163等

三种业务模式，减少业务无关的投入成本

####  1 OpenStack 定义

Open stack is a cloud operating system that controls large pools of compute,storage networking resources throughout a data center

管理大量资源池的云操作系统，资源池包括，计算，管理，存储，管理员通过web平台管理系统进行管理，并提供 web 接口让第三方应用来访问这个资源池

管理数据中心的资源，并进行分配

是亚马孙云计算的山寨版本；字面理解：开放，堆积，开源软件堆积的集合；云的具体实现；一个集成框架，做定性开发，比如计算，web er 和 kemer,搭建计算资源池

####  2 OpenStack 核心项目

- Nova （Computer-server）：计算资源生命周期的管理组件
- Neutron(NetWork server)：提供云计算环境下的虚拟网络功能
- cinder ：管理计算实例所使用的块级存储
- Swift：对象存储，用于永久类型的静态数据长期存储
- Glance ：提供虚拟镜像的发现，注册，获取服务
- Keystone：提供用户信息管理，为其他组件提供认证服务
- Horizon：用于管理，控制 OpenStack 服务的 web 控制面板

各组件之间关联，通过 Horizon web 界面创建一个虚拟机为例：

用户认证后--Horizon --创建虚拟机VMs--  镜像文件的安装glance--Nova--swift进行文件存储--虚拟机创建完成后，加入到已存在的集群中--neutron提供网络服务---cinder挂载硬盘，提供内存空间，块级存储

技术：大数据和云计算发展

华为使用 OpenStack  国外主要是，惠普公司

发展趋势：从 ISSA 到 PAAS，基础设施器服务到大数据项目沙哈拉；技术标准化；互联互通

####   3  OpenStack 架构

OpenStack 开发语言：Python  高内聚，低耦合

每个核心组件都会提供一个 API ，内部通过消息队列进行交互

无中心；分布式部署；插件化可配置（Docker 管理）

![架构设计特点](C:\Users\Administrator\Desktop\openstack安装配置\架构设计特点.png)

![openstack 架构职责](C:\Users\Administrator\Desktop\openstack安装配置\openstack 架构职责.png)



优点： 部署灵活，易扩展，易集成

![对比Docker](C:\Users\Administrator\Desktop\openstack安装配置\对比Docker.png)

![对比cloudstack](C:\Users\Administrator\Desktop\openstack安装配置\对比cloudstack.png)





####   4 OpenStack 底层技术

CPU 中央处理器

特权级：Ring 0~3 从高到底

内核态与用户态

计算虚拟化：解决多个CPU怎么虚拟化问题

虚拟化管理程序：hypervisor(VMM) 一种运行在基础物理服务器和操作系统之间的中间软件层，可允许多个操作系统和应用共享硬件

两种实现方式：半虚拟化只能Linux 用过hypervisor 转义实现； 全虚拟化

硬件辅助全虚拟化： inter VT 和 AMD-V 技术；查询CPU 是否支持： grep 'vmx' /proc/cpuinfo 或 'svm'

操作系统虚拟化：基于Linux namespace chroot cgroup，docker等

qemu：X86 架构，支持半虚拟化，软件

kvm：

![hypervisor 软件对比](C:\Users\Administrator\Desktop\hypervisor 软件对比.png)

libvirt：开源的C函数库，支持Linux下主流虚拟化管理程序，为各种虚拟化管理程序提供一套方便，可靠的编程接口

virt-manger（图形化）和 virtsh (命令行)

节点node：一个物理机器，可运行多个虚拟客户机

域 Domain：是hypervisor上运行的一个客户机操作系统实例，也叫实例 instance、客户机操作系统 guest os 虚拟机 virtual machine

网络虚拟化：OSI 七层模型

主机层 应用，表示，会话，传输 

网络层：链路层，物理层

SDN（软件定义网络）：软件驱动底层硬件，架构：应用，控制，基础设施层

SDN 功能单一，价格低；升级简单；业务响应快

open vswitch  OVS 虚拟化交换机

![open vswitch重要组件](C:\Users\Administrator\Desktop\open vswitch重要组件.png)



####    5  OpenStack 通用组件

Python 2.7 开发

![OpenStack 安装](C:\Users\Administrator\Desktop\OpenStack 安装.png)

REST

一种架构风格，核心是面向资源，基于 http 协议

http 常见四种操作：GET  POST  PUT  DELETE 分别：获取资源，新建资源，更新或新建资源，删除资源

WSGI

web 服务网关接口，包括：server middleware application

![WSGI](C:\Users\Administrator\Desktop\WSGI.png)

paste Deployment  PD工具包

![paste deployment工具包](C:\Users\Administrator\Desktop\paste deployment工具包.png)

数据库：Maria DB MySQL 的分支版本，包括 API 和客户端协议

![Maria DB 数据库](C:\Users\Administrator\Desktop\Maria DB 数据库.png)

Rabbit MQ

![Rabbit MQ](C:\Users\Administrator\Desktop\Rabbit MQ.png)



####  6 安装配置

安装部署文档

1 数据库服务---修改配置文件

2 MQ 服务  -- 创建用户 默认端口5672 查看MQ状态 ： rabbitmqctl status

3-4 keystone 创建一个名为keystone的数据库，创建一个同名用户；允许其它服务器访问 ---  文件备份 cp keystone.conf keystone.conf.bak

查看配置文件是否正确：grep '^[^#]' keystone.conf

5 初始化数据库并创建表

6 站点配置，两个端口分流

7 下载程序模板

