## 基础

### 网站

中文：https://springdoc.cn/spring-cloud-openfeign/

官网：https://docs.spring.io/spring-cloud-openfeign/reference/spring-cloud-openfeign.html

github：https://github.com/spring-cloud/spring-cloud-openfeign

### 简介

Feign是一个**声明性web服务客户端**。它使编写web服务客户端变得更容易。使用Feign创建一个接口并对其进行注释。它具有可插入的注释支持，包括Feign注释和JAX-RS注释。Feign还支持可插拔编码器和解码器。Spring Cloud添加了对Spring MVC注释的支持，以及对使用Spring Web中默认使用的HttpMessageConverter的支持。Spring Cloud集成了Eureka、Spring Cloud CircuitBreaker以及Spring Cloud LoadBalancer，以便在使用Feign时提供负载平衡的http客户端。

### 进步之处

在使用**SpringCloud LoadBalancer**+RestTemplate时，利用RestTemplate对http请求的封装处理形成了一套模版化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。所以，OpenFeign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。在OpenFeign的实现下，我们只需创建一个接口并使用注解的方式来配置它(在一个微服务接口上面标注一个@FeignClient注解即可)，即可完成对服务提供方的接口绑定，统一对外暴露可以被调用的接口方法，大大简化和降低了调用客户端的开发量，也即由服务提供者给出调用接口清单，消费者直接通过OpenFeign调用即可。

 OpenFeign同时还集成SpringCloud LoadBalancer，可以在使用OpenFeign时提供Http客户端的负载均衡，也可以集成阿里巴巴Sentinel来提供熔断、降级等功能。而与SpringCloud LoadBalancer不同的是，通过OpenFeign只需要定义服务绑定接口且以声明式的方法，优雅而简单的实现了服务调用。

## 应用

### 基础步骤

​	0、前提：已经有服务提供者，服务已经绑定到服务中心

​	1、添加依赖

```
<!--openfeign-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

​	2、配置API（可将其编写在工具包）

类注解指定服务名：@FeignClient(value = "cloud-payment-service")

根据注解调用不同接口

```
package com.atguigu.cloud.apis;

import com.atguigu.cloud.entities.PayDTO;
import com.atguigu.cloud.resp.ResultData;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
@FeignClient(value = "cloud-payment-service")
//@FeignClient(value = "cloud-gateway")
public interface PayFeignApi
{
    /**
     * 新增一条支付相关流水记录
     * @param payDTO
     * @return
     */
    @PostMapping(value = "/pay/add")
    public ResultData addPay(@RequestBody PayDTO payDTO);

    /**
     * 按照主键记录查询支付流水信息
     * @param id
     * @return
     */
    @GetMapping(value = "/pay/get/{id}")
    public ResultData getPayInfo(@PathVariable("id") Integer id);

    /**
     * openfeign天然支持负载均衡演示
     * @return
     */
    @GetMapping(value = "/pay/get/info")
    public String mylb();

    /**
     * Resilience4j CircuitBreaker 的例子
     * @param id
     * @return
     */
    @GetMapping(value = "/pay/circuit/{id}")
    public String myCircuit(@PathVariable("id") Integer id);

    /**
     * Resilience4j Bulkhead 的例子
     * @param id
     * @return
     */
    @GetMapping(value = "/pay/bulkhead/{id}")
    public String myBulkhead(@PathVariable("id") Integer id);


    /**
     * Resilience4j Ratelimit 的例子
     * @param id
     * @return
     */
    @GetMapping(value = "/pay/ratelimit/{id}")
    public String myRatelimit(@PathVariable("id") Integer id);


    /**
     * Micrometer(Sleuth)进行链路监控的例子
     * @param id
     * @return
     */
    @GetMapping(value = "/pay/micrometer/{id}")
    public String myMicrometer(@PathVariable("id") Integer id);

    /**
     * GateWay进行网关测试案例01
     * @param id
     * @return
     */
    @GetMapping(value = "/pay/gateway/get/{id}")
    public ResultData getById(@PathVariable("id") Integer id);

    /**
     * GateWay进行网关测试案例02
     * @return
     */
    @GetMapping(value = "/pay/gateway/info")
    public ResultData<String> getGatewayInfo();

}
```

​	3、main启动

```java
@SpringBootApplication
@EnableDiscoveryClient //该注解用于向使用consul为注册中心时注册服务
@EnableFeignClients//启用feign客户端,定义服务+绑定接口，以声明式的方法优雅而简单的实现服务调用
public class MainOpenFeign80
{
    public static void main(String[] args)
    {
        SpringApplication.run(MainOpenFeign80.class,args);
    }
}
```

​	4、调用API

```java
package com.atguigu.cloud.controller;


import com.atguigu.cloud.apis.PayFeignApi;
import com.atguigu.cloud.entities.PayDTO;
import com.atguigu.cloud.resp.ResultData;
import jakarta.annotation.Resource;
import org.springframework.web.bind.annotation.*;

@RestController
public class OrderController
{
    @Resource
    private PayFeignApi payFeignApi;

    @PostMapping("/feign/pay/add")
    public ResultData addOrder(@RequestBody PayDTO payDTO)
    {
        System.out.println("第一步：模拟本地addOrder新增订单成功(省略sql操作)，第二步：再开启addPay支付微服务远程调用");
        ResultData resultData = payFeignApi.addPay(payDTO);
        return resultData;
    }

    @GetMapping("/feign/pay/get/{id}")
    public ResultData getPayInfo(@PathVariable("id") Integer id)
    {
        System.out.println("-------支付微服务远程调用，按照id查询订单支付流水信息");
        ResultData resultData = payFeignApi.getPayInfo(id);
        return resultData;
    }

    /**
     * openfeign天然支持负载均衡演示
     *
     * @return
     */
    @GetMapping(value = "/feign/pay/mylb")
    public String mylb()
    {
        return payFeignApi.mylb();
    }

}
```

### 超时处理

- `connectTimeout` 防止因服务器处理时间过长而阻塞调用者。
- `readTimeout` 从连接建立时开始应用，当返回响应的时间过长时就会被触发。

#### 项目全局配置

```
spring:
  cloud:
    openfeign:
      client:
        config:
          default:
            #连接超时时间
            connectTimeout: 3000
            #读取超时时间
            readTimeout: 3000
```

#### 项目单个服务配置

```yml
spring:
  cloud:
    openfeign:
      client:
        config:
          cloud-payment-service:
            #连接超时时间
            connectTimeout: 5000
            #读取超时时间
            readTimeout: 5000
```

如果存在多个配置，局部会覆盖全局

### 重试机制

重试配置：feign默认不重试

```
@Configuration
public class OpenFeignConfig {
    @Bean
    public Retryer myRetryer()
    {
        //return Retryer.NEVER_RETRY; //Feign默认配置是不走重试策略的
        //最大请求次数为3(1+2)，初始间隔时间为100ms，重试间最大间隔时间为1s
        return new Retryer.Default(100,1,3);
    }
}
```

- 初始间隔时间为 100 毫秒：在第一次重试请求失败后，等待 100 毫秒后进行第二次请求。
- 重试最大间隔时间为 1 秒：重试的间隔时间会逐渐增加，但不会超过 1 秒。
- 最大请求次数为 3：允许进行的最大重试次数为 3 次（包括第一次请求和两次重试）。

### HttpClient5

#### 介绍

OpenFeign默认使用JDK自带的HttpURLConnection发送HTTP请求，由于默认HttpURLConnection没有连接池、性能和效率比较低，如果采用默认，性能上不是最强的

功能丰富：HttpClient 提供了更加丰富和完善的功能，例如连接池管理、请求重试、HTTPS 支持、认证机制等，相比 JDK 自带的 HttpURLConnection，功能更加强大。

性能优越：HttpClient 在性能方面通常优于 JDK 自带的 HttpURLConnection。它通过复用连接、支持异步请求等方式可以提高性能，并且在高并发场景下表现更稳定。

配置灵活：HttpClient 提供了丰富的配置选项，可以轻松地对连接超时、请求超时、最大连接数等参数进行调整，而 JDK 自带的 HttpURLConnection 的配置相对较为局限。

易于扩展：HttpClient 本身具有良好的设计和扩展性，支持插件机制、自定义拦截器等，可以灵活地扩展和定制客户端行为。

#### 步骤

1、添加依赖

```
<!-- httpclient5-->
<dependency>
    <groupId>org.apache.httpcomponents.client5</groupId>
    <artifactId>httpclient5</artifactId>
    <version>5.3</version>
</dependency>
<!-- feign-hc5-->
<dependency>
    <groupId>io.github.openfeign</groupId>
    <artifactId>feign-hc5</artifactId>
    <version>13.1</version>
</dependency>
```

2、开启配置

```
#  Apache HttpClient5 配置开启
spring:
  cloud:
    openfeign:
      httpclient:
        hc5:
          enabled: true
```

### 请求/响应压缩

**对请求和响应进行GZIP压缩**

Spring Cloud OpenFeign支持对请求和响应进行GZIP压缩，以减少通信过程中的性能损耗。

通过下面的两个参数设置，就能开启请求与相应的压缩功能：

spring.cloud.openfeign.compression.request.enabled=true

spring.cloud.openfeign.compression.response.enabled=true

**细粒度化设置**

对请求压缩做一些更细致的设置，比如下面的配置内容指定压缩的请求数据类型并设置了请求压缩的大小下限，

只有超过这个大小的请求才会进行压缩：

spring.cloud.openfeign.compression.request.enabled=true

spring.cloud.openfeign.compression.request.mime-types=text/xml,application/xml,application/json #触发压缩数据类型

spring.cloud.openfeign.compression.request.min-request-size=2048 #最小触发压缩的大小

### 日志打印

意义：对Feign接口的调用情况进行监控和输出

日志级别

```
NONE：默认的，不显示任何日志；

BASIC：仅记录请求方法、URL、响应状态码及执行时间；

HEADERS：除了 BASIC 中定义的信息之外，还有请求和响应的头信息；

FULL：除了 HEADERS 中定义的信息之外，还有请求和响应的正文及元数据。
```

步骤：

1、配置文件

```
logging:
  level:
    com:
      atguigu:
        cloud:
          apis:
            PayFeignApi: debug
```

2、配置类

```
@Bean
Logger.Level feignLoggerLevel() {
    return Logger.Level.FULL;
}
```

## 原理