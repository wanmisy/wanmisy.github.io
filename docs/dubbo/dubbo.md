# 概念&架构

## 服务发现
消费端自动发现服务地址列表的能力，是微服务框架需要具备的关键能力，借助于自动化的服务发现，微服务之间可以在无需感知对端部署位置与 IP 地址的情况下实现通信。  
实现服务发现的方式有很多种，Dubbo 提供的是一种 Client-Based 的服务发现机制，通常还需要部署额外的第三方注册中心组件来协调服务发现过程，如常用的 Nacos、Consul、Zookeeper 等  

Dubbo 基于消费端的自动服务发现能力，其基本工作原理如下图：

![Dubbo 基于消费端的自动服务发现能力工作原理](https://dubbo.apache.org/imgs/architecture.png)

服务发现的一个核心组件是注册中心，Provider 注册地址到注册中心，Consumer 从注册中心读取和订阅 Provider 地址列表。 因此，要启用服务发现，需要为 Dubbo 增加注册中心配置：  
以 dubbo-spring-boot-starter 使用方式为例，增加 registry 配置  
```(properties)
# application.properties
dubbo
 registry
  address: zookeeper://127.0.0.1:2181
```

## 部署架构（注册中心 配置中心 元数据中心）

三大中心化组件  
* 注册中心。  
协调 Consumer 与 Provider 之间的地址注册与发现
* 配置中心。  
存储 Dubbo 启动阶段的全局配置，保证配置的跨环境共享与全局一致性。负责服务治理规则（路由规则、动态配置等）的存储与推送。
* 元数据中心。
接收 Provider 上报的服务接口元数据，为 Admin 等控制台提供运维能力（如服务测试、接口文档等）。作为服务发现机制的补充，提供额外的接口/方法级别配置信息的同步能力，相当于注册中心的额外扩展
![Dubbo 微服务组件与各个中心的交互过程](https://dubbo.apache.org/imgs/v3/concepts/threecenters.png)