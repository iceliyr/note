## 基础

### consul相关网站

官网：https://www.consul.io/

github官网：https://github.com/spring-cloud/spring-cloud-consul

springcloud官网：https://spring.io/projects/spring-cloud-consul#learn

### consul简介

`Consul`是HashiCorp公司推出的开源工具，Consul由Go语言开发，部署起来非常容易，只需要极少的可执行程序和配置文件，具有绿色、轻量级的特点。`Consul`是`分布式`的、`高可用`的、 `可横向扩展`的用于实现分布式系统的服务发现与配置

### 服务注册与发现中心

#### 特点

CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求。

因此，根据 CAP 原理将 NoSQL 数据库分成了满足 CA 原则、满足 CP 原则和满足 AP 原则三 大类：

​	CA - 单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大

​	CP - 满足一致性，分区容忍必的系统，通常性能不是特别高

​	AP - 满足可用性，分区容忍性的系统，通常可能对一致性要求低一

#### 不同注册中心比较	

#### consul作用

服务发现（Service Discovery）：`Consul`提供了通过DNS或者HTTP接口的方式来注册服务和发现服务。一些外部的服务通过Consul很容易的找到它所依赖的服务。

健康检查（Health Checking）：Consul的Client可以提供任意数量的健康检查，既可以与给定的服务相关联(“webserver是否返回200 OK”)，也可以与本地节点相关联(“内存利用率是否低于90%”)。操作员可以使用这些信息来监视集群的健康状况，服务发现组件可以使用这些信息将流量从不健康的主机路由出去。

Key/Value存储：应用程序可以根据自己的需要使用Consul提供的Key/Value存储。 Consul提供了简单易用的HTTP接口，结合其他工具可以实现动态配置、功能标记、领袖选举等等功能。

安全服务通信：Consul可以为服务生成和分发TLS证书，以建立相互的TLS连接。意图可用于定义允许哪些服务通信。服务分割可以很容易地进行管理，其目的是可以实时更改的，而不是使用复杂的网络拓扑和静态防火墙规则。

多数据中心：Consul支持开箱即用的多数据中心. 这意味着用户不需要担心需要建立额外的抽象层让业务扩展到多个区域。



## 应用

### 基础步骤

#### 1、安装运行

​	1、下载安装包：https://developer.hashicorp.com/consul/install?ajs_aid=13caf34a-d10f-4146-889a-d000dfe30901&product_intent=consul

​	2、启动consul：控制台输入：consul agent -dev

​	3、访问：http://localhost:8500

#### 2、服务部署

​	1、引入依赖：pom.xml

```xml
<!--SpringCloud consul discovery -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>
```

​	2、全局BP：application.yml

```yml
spring:
  application:
    name: cloud-payment-service
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://192.168.126.131:3306/springcloud2024?characterEncoding=utf8&useSSL=false&serverTimezone=GMT%2B8&rewriteBatchedStatements=true&allowPublicKeyRetrieval=true
    username: win
    password: 123456
  ####Spring Cloud Consul for Service Discovery
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

​	3、main类添加注解

```java
@SpringBootApplication
@MapperScan("com.atguigu.cloud.mapper") //import tk.mybatis.spring.annotation.MapperScan;
@EnableDiscoveryClient
public class Main8001
{
    public static void main(String[] args)
    {
        SpringApplication.run(Main8001.class,args);
    }
}
```

#### 3、服务调用

​	1、添加依赖

```xml
<!--SpringCloud consul discovery -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>
```

​	2、全局配置

```yml
spring:
  application:
    name: cloud-consumer-order
  ####Spring Cloud Consul for Service Discovery
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        prefer-ip-address: true #优先使用服务ip进行注册
        service-name: ${spring.application.name}
```

​	3、main类添加注解

```java
@SpringBootApplication
@EnableDiscoveryClient
public class Main80
{
    public static void main(String[] args)
    {
        SpringApplication.run(Main80.class,args);
    }
}
```

​	4、配置类

```java
@Configuration
@LoadBalancerClient(value = "cloud-payment-service",configuration = RestTemplateConfig.class)
public class RestTemplateConfig
{
    @Bean
    @LoadBalanced //使用@LoadBalanced注解赋予RestTemplate负载均衡的能力
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }

    @Bean
    ReactorLoadBalancer<ServiceInstance> randomLoadBalancer(Environment environment,
                                                            LoadBalancerClientFactory loadBalancerClientFactory) {
        String name = environment.getProperty(LoadBalancerClientFactory.PROPERTY_NAME);

        return new RandomLoadBalancer(loadBalancerClientFactory.getLazyProvider(name, ServiceInstanceListSupplier.class), name);
    }
}
```

​	5、服务调用

```java
public static final String PaymentSrv_URL = "http://cloud-payment-service";//服务注册中心上的微服务名称
@Resource
private RestTemplate restTemplate;

@GetMapping(value = "/consumer/pay/add")
public ResultData addOrder(PayDTO payDTO)
{
    return restTemplate.postForObject(PaymentSrv_URL + "/pay/add", payDTO, ResultData.class);
}

@GetMapping(value = "/consumer/pay/get/{id}")
public ResultData getPayInfo(@PathVariable("id") Integer id)
{
    return restTemplate.getForObject(PaymentSrv_URL + "/pay/get/"+id,ResultData.class,id);
}

@GetMapping(value = "/consumer/pay/get/info")
private String getInfoByConsul()
{
    return restTemplate.getForObject(PaymentSrv_URL + "/pay/get/info", String.class);
}
```

### 系统级配置（bootstrap.yml）

#### 介绍

applicaiton.yml是用户级的资源配置项

bootstrap.yml是系统级的，优先级更加高

Spring Cloud会创建一个“Bootstrap Context”，作为Spring应用的`Application Context`的父上下文。初始化的时候，`Bootstrap Context`负责从外部源加载配置属性并解析配置。这两个上下文共享一个从外部获取的`Environment`。

`Bootstrap`属性有高优先级，默认情况下，它们不会被本地配置覆盖。 `Bootstrap context`和`Application Context`有着不同的约定，所以新增了一个`bootstrap.yml`文件，保证`Bootstrap Context`和`Application Context`配置的分离。

 application.yml文件改为bootstrap.yml,这是很关键的或者两者共存

因为bootstrap.yml是比application.yml先加载的。bootstrap.yml优先级高于application.yml

#### 步骤

1、添加依赖

```xml
<!--SpringCloud consul config-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-config</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bootstrap</artifactId>
</dependency>
```

2、添加配置（bootstrap.yml）

```yaml
spring:
  application:
    name: cloud-payment-service
    ####Spring Cloud Consul for Service Discovery
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
      config:
        profile-separator: '-' # default value is ","，we update '-'
        format: YAML
```

3、consul配置

（1）创建相应服务文件夹（名字与服务一样，可以选择不同环境）![image-20240320150759901](F:\Typora\SpringCloud\consul\image-20240320150759901.png)

（2）创建key为data，并配置值![image-20240320150928565](F:\Typora\SpringCloud\consul\image-202403201509285651.png)

4、application配置

| **spring**:  **profiles**:   **active**: dev *#* *多环境配置加载内容dev* |
| ------------------------------------------------------------ |
| **spring**:  **profiles**:   **active**: prod *#* *多环境配置加载内容prod* |
| **spring**:  **profiles**:   **active**: *#* *多环境配置加载内容默认* |

5、bootstrap配置

```
server:
  port: 8001

# ==========applicationName + druid-mysql8 driver===================
spring:
  application:
    name: cloud-payment-service

  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://192.168.126.131:3306/springcloud2024?characterEncoding=utf8&useSSL=false&serverTimezone=GMT%2B8&rewriteBatchedStatements=true&allowPublicKeyRetrieval=true
    username: win
    password: 123456
  ####Spring Cloud Consul for Service Discovery
  profiles:
    active:

# ========================mybatis===================
mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.atguigu.cloud.entities
  configuration:
    map-underscore-to-camel-case: true
```

6、调用配置

```java
@GetMapping(value = "/pay/get/info")
private String getInfoByConsul(@Value("${atguigu.info}") String atguiguInfo)
{
    return "atguiguInfo: "+atguiguInfo+"\t"+"port: "+port;
}
```

### 后台启动与数据持久化

#### 基础步骤

1、新建文件夹保存数据及新建bat执行文件![image-20240321155754543](.\consul\image-20240321155754543.png)

2、bat执行文件内容

```
`@echo.服务启动......` 

`@echo off` 

`@sc create Consul binpath= "D:\devSoft\consul_1.17.0_windows_386\consul.exe agent -server -ui -bind=127.0.0.1 -client=0.0.0.0 -bootstrap-expect 1 -data-dir D:\devSoft\consul_1.17.0_windows_386\mydata  "`

`@net start Consul`

`@sc config Consul start= AUTO` 

`@echo.Consul start is OK......success`

`@pause`
```

3、管理员身份执行bat文件

## 原理