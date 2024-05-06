## 基础

### 负载均衡与服务调用

负载均衡：简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA（高可用），常见的负载均衡有软件Nginx，LVS，硬件 F5等

### 负载均衡与服务调用相关组件

#### Ribbon

Spring Cloud Ribbon是基于Netflix Ribbon实现的一套*客户端*    负载均衡的工具。

简单的说，Ribbon是Netflix发布的开源项目，主要功能是提供客户端的软件负载均衡算法和服务调用。Ribbon客户端组件提供一系列完善的配置项如连接超时，重试等。简单的说，就是在配置文件中列出Load Balancer（简称LB）后面所有的机器，Ribbon会自动的帮助你基于某种规则（如简单轮询，随机连接等）去连接这些机器。我们很容易使用Ribbon实现自定义的负载均衡算法。

#### Loadbalancer

官网：https://docs.spring.io/spring-cloud-commons/reference/spring-cloud-commons/loadbalancer.html

定义： Spring Cloud LoadBalancer是由SpringCloud官方提供的一个开源的、简单易用的**客户端负载均衡器**，它包含在SpringCloud-commons中用它来替换了以前的Ribbon组件。相比较于Ribbon，SpringCloud LoadBalancer不仅能够支持RestTemplate，还支持WebClient（WeClient是Spring Web Flux中提供的功能，可以实现响应式异步请求）

#### Ribbon与Loadbalancer区别

Ribbon和Spring Cloud Loadbalancer都是用于服务间通信和负载均衡的工具，但它们在使用方式和特点上有一些不同。Ribbon是一个轻量级的客户端负载均衡器，主要用于访问外部服务，如Amazon DynamoDB或RESTful Web服务。它提供了简单而灵活的负载均衡策略，以及请求超时、重试机制等高级功能。另一方面，Spring Cloud Loadbalancer是一个更通用的负载均衡解决方案，与Spring Cloud生态系统中其他组件集成良好。它支持多种负载均衡策略，并提供了对容错和可观察性的内置支持。
随着Spring Cloud的不断发展，越来越多的开发者和企业开始采用Spring Cloud Loadbalancer作为其服务间通信和负载均衡的解决方案。这主要是因为Spring Cloud Loadbalancer与Spring Cloud的其他组件（如Ribbon、Feign等）具有良好的集成性，使得开发人员可以更轻松地构建微服务应用程序。此外，Spring Cloud Loadbalancer还提供了更多的功能和灵活性，如动态配置、健康检查等，以满足不断变化的服务需求。
然而，这并不意味着Ribbon会被完全淘汰。Ribbon仍然是一个强大而灵活的负载均衡器，对于许多用例来说仍然是理想的选择。它提供了许多高级功能，如自定义负载均衡策略、请求超时管理等，这些功能在某些场景下可能是必需的。此外，Ribbon已经有了庞大的用户基础和丰富的社区支持，这意味着它将继续得到维护和改进。
因此，对于开发者来说，选择Ribbon还是Spring Cloud Loadbalancer取决于具体的需求和场景。如果需要与Spring Cloud生态系统深度集成、易于管理和配置的负载均衡解决方案，那么Spring Cloud Loadbalancer可能是更好的选择。如果需要更多的自定义选项和高级功能，那么Ribbon可能仍然是一个合适的选择。
总的来说，虽然Ribbon可能会被Spring Cloud Loadbalancer所取代，但它仍然是一个强大而灵活的工具，在许多场景下仍然具有价值。随着技术的发展和社区的演变，我们可能会看到更多的负载均衡解决方案出现，以满足不断变化的服务需求。因此，持续关注新技术的发展，并根据具体需求进行选择将是至关重要的。



## 应用（LoadBalancer）

### 基础步骤

1、添加依赖

```
<!--loadbalancer-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-loadbalancer</artifactId>
</dependency>
```

2、添加配置

3、配置类

```
@Configuration
@LoadBalancerClient(
        //下面的value值大小写一定要和consul里面的名字一样，必须一样
        value = "cloud-payment-service",configuration = RestTemplateConfig.class)
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

4、controller类调用服务

```
public static final String PaymentSrv_URL = "http://cloud-payment-service";//服务注册中心上的微服务名称
@Resource
private RestTemplate restTemplate;

@GetMapping(value = "/consumer/pay/add")
public ResultData addOrder(PayDTO payDTO)
{
    return restTemplate.postForObject(PaymentSrv_URL + "/pay/add", payDTO, ResultData.class);
}
```

## 原理

### 负载均衡算法

#### 轮询（默认）

#### 随机

```
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

