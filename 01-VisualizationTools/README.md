# 01-VisualizationTools

> 目录:[https://github.com/dolyw/DockerStudy](https://github.com/dolyw/DockerStudy)

#### 项目目录

##### Rancher的安装使用: [https://github.com/dolyw/DockerStudy/blob/master/01-VisualizationTools/Rancher.md](https://github.com/dolyw/DockerStudy/blob/master/01-VisualizationTools/Rancher.md)

##### Portainer的安装使用: [https://github.com/dolyw/DockerStudy/blob/master/01-VisualizationTools/Portainer.md](https://github.com/dolyw/DockerStudy/blob/master/01-VisualizationTools/Portainer.md)

#### Docker的界面可视化

谈及Docker，避免不了需要熟练的记住好多命令及其用法，对于熟悉Shell、技术开发人员而言，还是可以接受的，熟练之后，命令行毕竟是很方便的，便于操作及脚本化

但对于命令行过敏、非技术人员，进行Docker部署、管理是比较头疼的，学习成本是很高的

而市面上的可视化管理工具也是很多的，各有优缺点，就DockerUI、Shipyard、Rancher、Portainer做一下对比

#### (一)DockerUI

**优点**

* 支持Container批量操作
* 支持Image管理(虽然比较薄弱)

**缺点**

* 不支持多主机，多环境
* 管理平台无登录认证机制

**结论**

* Web管理平台无登陆认证机制，考虑到使用过程中人员管理、权限管理等因素，很难留用，故弃之，个人临时使用可以

#### (二)Shipyard

**优点**

* 支持镜像管理、容器管理
* 支持控制台命令
* 容器资源消耗监控
* 支持集群Swarm，可以随意增加节点
* 支持控制用户管理权限，可以设置某个容器对某个用户只读、管理权限
* 有汉化版

**缺点**

* 启动容器较多，占用每个节点的一部分资源
* 镜像包较大，1个多G
* 2016年已停止维护，后期使用风险较高

**结论**

* Shipyard整个功能强大，能够满足使用，但镜像很大，消耗资源较大，而且2016年已停止维护，后期使用过程中出现问题，难以把控

#### (三)Rancher

**优点**

* 支持多种调度器
* 通过环境模板，很容易地创建和部署Cattle、Swarm、K8S、Mesos容器集群管理调度平台
* 管理主机集群。

**缺点**

* 镜像管理功能薄弱，无镜像导入、导出功能，镜像只能通过镜像库获取。

**结论**

* 镜像管理功能薄弱，无镜像导入、导出功能，镜像只能通过镜像库获取。如无镜像导入、导出需求，可作为不二之选

#### (四)Portainer

**优点**

* 支持容器管理、镜像管理(导入、导出)
* 轻量级，消耗资源少
* 基于Docker Api，安全性高，可指定Docker Api端口，支持TLS证书认证
* 支持权限分配
* 支持集群。
* Github上目前持续维护更新。

**缺点**

* N/A

**结论**

* Portainer功能完善，目前持续维护更新，最终我选择了它，作为Docker管理工具

#### 搭建参考

1. 感谢xcbeyond的Docker可视化管理工具对比(DockerUI、Shipyard、Rancher、Portainer): [https://blog.csdn.net/xcbeyond/article/details/82859903](https://blog.csdn.net/xcbeyond/article/details/82859903)


