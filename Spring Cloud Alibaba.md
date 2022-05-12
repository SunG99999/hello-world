# Spring Cloud Alibaba

## 一、概述

Spring Cloud Alibaba致力于提供微服务开发的一战式解决方案。此项目包含开发微服务架构的必需组件，方便开发者通过Spring Cloud 编程模型轻松使用这些组件来开发微服务架构。

### 1.1 Spring Cloud 各套实现对比

|               |     Spring Cloud  Alibaba     |     Spring Cloud  Netflix     |     Spirng Cloud 官方      | Spring Cloud  Zookeeper | Spring Cloud  Consul | Spring Cloud  Kub ernetes |
| :-----------: | :---------------------------: | :---------------------------: | :------------------------: | :---------------------: | :------------------: | :-----------------------: |
|  分布式配置   |         Nacos Config          |           Archaius            |    Spring Cloud Config     |        Zookeeper        |        Consul        |         ConfigMap         |
| 服务注册/发现 |        Nacos Discovery        |            Eureka             |             -              |        Zookeeper        |        Consul        |        Api Server         |
|   服务熔断    |           Sentinel            |            Hystrix            |             -              |            -            |          -           |             -             |
|   服务调用    |           Dubbo RPC           |             Feign             | OpenFeign     RestTemplate |            -            |          -           |             -             |
|   服务路由    |        Dubbo + Servlet        |             Zuul              |    Spring Cloud Gateway    |            -            |          -           |             -             |
|  分布式消息   |      SCS RocketMQ Binder      |               -               |    SCS RabiitMQ Binder     |            -            |  SCS Consul Binder   |             -             |
|   消息总线    |         Rocket MQ Bus         |               -               |        RabbitMQ Bus        |            -            |      Consul Bus      |             -             |
|   负载均衡    |       Dobbo LoadBalance       |            Ribbon             |             -              |            -            |          -           |             -             |
|  分布式事务   |             Seata             |               -               |             -              |            -            |          -           |             -             |
|    Sidecar    | Spring Cloud Alibaba  Sidecar | Spring Cloud Netflix  Sidecar |             -              |            -            |          -           |             -             |

### 1.2 Spring Cloud Aliaba 搭建

组件版本关系

# 二、Alibaba微服务组件Nacos注册中心

### 2.1 什么是Nacos

一个更易于构建云原生应用的动态服务发现（Nacos Discovery）、服务配置（Nacos Config）和服务管理平台。

Nacos的关键特性包括：

- 服务发现和服务健康监测
- 动态配置服务
- 动态DNS服务
- 服务及其元数据管理

### 2.2 核心功能

服务注册：Nacos Client会通过发生REST请求的方式向Nacos Sever注册自己的服务，提供自身的元数据，比如IP地址、端口等信息。Nacos Server接收到注册请求后，就会把这些元数据信息存储在一个双层的内存Map中。

服务心跳：在服务注册后，Nacos Client会维护一个定时心跳来持续通知Nacos Server，说明服务一直处于可用状态，防止被剔除。默认5s中发送一次心跳。

服务同步：Nacos Server集群之间会互相同步服务实例，用来保证服务信息的一致性。leader raft算法

服务发现：服务消费者（Nacos Client）在调用服务提供者的服务时，会发送一个REST请求给Nacos Server，获取上面注册的服务清单，并且缓存在Nacos Client本地，同时会在Nacos Client 本地开启一个定时任务拉取服务器最新的注册表信息更新到本地缓存。

服务健康监测：Nacos Server会开启一个定时任务用来检查注册服务实例的健康状态，对应超过15s没有收到客户端心跳的实例会将它的healthy属性设置为false（客户端服务发现时不会发现），如果某个实例超过30s没有收到心跳，直接剔除该实例（被删除的实例如果恢复发送心跳则会重新注册）。

### 2.3 Nacos 注册中心与其他注册中心对比

|                   | Nacos                       | Eureka       | Consul            | Zookeeper   |
| ----------------- | --------------------------- | ------------ | ----------------- | ----------- |
| 一致性协议        | CP+AP                       | AP           | CP                | CP          |
| 健康检查          | TCP/HTTP/MYSQL/Client  Beat | Client  Beat | TCP/HTTP/gPRC/Cmd | Keep  Alive |
| 负载均衡策略      | 权重/metadata/Selector      | Ribbon       | Fabio             | -           |
| 雪崩保护          | 有                          | 有           | 无                | 无          |
| 自动注销实例      | 支持                        | 支持         | 支持              | 支持        |
| 访问协议          | HTTP/DNS                    | HTTP         | HTTP/DNS          | TCP         |
| 监听支持          | 支持                        | 支持         | 支持              | 支持        |
| 多数据中心        | 支持                        | 支持         | 支持              | 不支持      |
| 跨注册中心        | 支持                        | 不支持       | 支持              | 不支持      |
| Spring Cloud 集成 | 支持                        | 支持         | 支持              | 支持        |
| Dobbo集成         | 支持                        | 不支持       | 支持              | 支持        |
| K8s集成           | 支持                        | 不支持       | 支持              | 不支持      |

### 2.4 Nacos 部署

单机版：

```bash
./startup.sh -m standalone     # 单据版启动
```





















