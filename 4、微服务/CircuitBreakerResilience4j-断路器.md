## 基础

### 分布式系统面临的问题

复杂分布式体系结构中的应用程序有数十个依赖关系，每个依赖关系在某些时候将不可避免地失败

### 服务雪崩

多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其它的微服务，这就是所谓的“扇出”。如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应”.

对于高流量的应用来说，单一的后端依赖可能会导致所有服务器上的所有资源都在几秒钟内饱和。比失败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张，导致整个系统发生更多的级联故障。这些都表示需要对故障和延迟进行隔离和管理，以便单个依赖关系的失败，不能取消整个应用程序或系统。

所以，通常当你发现一个模块下的某个实例失败后，这时候这个模块依然还会接收流量，然后这个有问题的模块还调用了其他的模块，这样就会发生级联故障，或者叫雪崩。

### 解决服务雪崩

#### 服务熔断

类比保险丝,保险丝闭合状态(CLOSE)可以正常使用,当达到最大服务访问后,直接拒绝访问跳闸限电(OPEN),此刻调用方会接受服务降级的处理并返回友好兜底提示
就是家里保险丝,从闭合CLOSE供电状态→跳闸OPEN打开状态

#### 服务降级

服务器忙,请稍后再试。
不让客户端等待并立刻返回一个友好提示,fallback

#### 服务限流

秒杀高并发等操作,严禁一窝蜂的过来拥挤,大家排队,一秒钟N个,有序进行

#### 服务限时

限定时间

#### 服务预热

#### 接近实时的监控

#### 兜底的处理动作

### Hystrix（已淘汰）

Hystrix是一个用于处理分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时、异常等，Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。

### 断路器

“断路器”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝），向调用方返回一个符合预期的、可处理的备选响应(FallBack)，而不是长时间的等待或者抛出调用方无法处理的异常，这样就保证了服务调用方的线程不会被长时间、不必要地占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩。

### CircuitBreaker（接口规范）

官网：https://spring.io/projects/spring-cloud-circuitbreaker

github：https://github.com/spring-cloud/spring-cloud-circuitbreaker

#### 简介

CircuitBreaker的目的是保护分布式系统免受故障和异常，提高系统的可用性和健壮性。
当一个组件或服务出现故障时，CircuitBreaker会迅速切换到开放OPEN状态(保险丝跳闸断电)，阻止请求发送到该组件或服务从而避免更多的请求发送到该组件或服务。这可以减少对该组件或服务的负载，防止该组件或服务进一步崩溃，并使整个系统能够继续正常运行。同时，CircuitBreaker还可以提高系统的可用性和健壮性，因为它可以在分布式系统的各个组件之间自动切换，从而避免单点故障的问题。

### resilience4j（实现CircuitBreaker规范）

github：https://github.com/resilience4j/resilience4j

中文：https://github.com/lmhmhl/Resilience4j-Guides-Chinese/blob/main/index.md

官网：https://resilience4j.readme.io/docs/getting-started

#### 简介

Resilience4j 是一个专为函数式编程设计的轻量级容错库。Resilience4j提供高阶函数(装饰器),以通过断路器、速率
限制器、重试或隔板增强任何功能接口、lambda表达式或方法引用。您可以在任何函数式接口、lambda表达式或方
法引用上堆叠多个装饰器。优点是您可以选择您需要的装饰器,而没有其他选择。

Resilience4j 2 需要 Java 17。

#### 作用

断路器

速率限制

舱壁

自动重试(同步和异步)

超时处理

结果缓存

## 原理

### CircuitBreaker

https://zhuanlan.zhihu.com/p/519720107

### 断路器

#### 三大状态

![image-20240322214949701](.\CircuitBreaker\image-20240322214949701.png)

断路器有三个普通状态:关闭(CLOSED)、开启(OPEN)、半开(HALF_OPEN),还有两个特殊状态:禁用
(DISABLED)、强制开启(FORCED_OPEN)。

1、当熔断器关闭时,所有的请求都会通过熔断器：
	如果失败率超过设定的阈值,熔断器就会从关闭状态转换到打开状态,这时所有的请求都会被拒绝。

​	当经过一段时间后,熔断器会从打开状态转换到半开状态,这时仅有一定数量的请求会被放入,并重新计
​	算失败率

​	如果失败率超过阈值,则变为打开状态,如果失败率低于阈值,则变为关闭状态。

2、断路器使用滑动窗口来存储和统计调用的结果。你可以选择基于调用数量的滑动窗口或者基于时间的滑动窗
口：

​	基于访问数量的滑动窗口统计了最近N次调用的返回结果。居于时间的滑动窗口统计了最近N秒的调用
回结果。

3、除此以外,熔断器还会有两种特殊状态:DISABLED(始终允许访问)和FORCED_OPEN(始终拒绝访问)：

​	这两个状态不会生成熔断器事件(除状态装换外),并且不会记录事件的成功或者失败。
​	退出这两个状态的唯一方法是触发状态转换或者重置熔断器。

## 应用（resilience4j）

### 断路器

#### 参数

| 参数                                         | 意义                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| failure-rate-threshold                       | 以百分比配置失败率峰值                                       |
| sliding-window-type                          | 断路器的滑动窗口期类型 可以基于“次数”（COUNT_BASED）或者“时间”（TIME_BASED）进行熔断，默认是COUNT_BASED。 |
| sliding-window-size                          | 若COUNT_BASED，则10次调用中有50%失败（即5次）打开熔断断路器；若为TIME_BASED则，此时还有额外的两个设置属性，含义为：在N秒内（sliding-window-size）100%（slow-call-rate-threshold）的请求超过N秒（slow-call-duration-threshold）打开断路器。 |
| slowCallRateThreshold                        | 以百分比的方式配置，断路器把调用时间大于slowCallDurationThreshold的调用视为慢调用，当慢调用比例大于等于峰值时，断路器开启，并进入服务降级。 |
| slowCallDurationThreshold                    | 配置调用时间的峰值，高于该峰值的视为慢调用。                 |
| permitted-number-of-calls-in-half-open-state | 运行断路器在HALF_OPEN状态下时进行N次调用，如果故障或慢速调用仍然高于阈值，断路器再次进入打开状态。 |
| minimum-number-of-calls                      | 在每个滑动窗口期样本数，配置断路器计算错误率或者慢调用率的最小调用数。比如设置为5意味着，在计算故障率之前，必须至少调用5次。如果只记录了4次，即使4次都失败了，断路器也不会进入到打开状态。 |
| wait-duration-in-open-state                  | 从OPEN到HALF_OPEN状态需要等待的时间                          |

#### 使用1（基于计数）

1、服务提供者及接口定义

2、添加依赖

```
<!--resilience4j-circuitbreaker-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-circuitbreaker-resilience4j</artifactId>
</dependency>
<!-- 由于断路保护等需要AOP实现，所以必须导入AOP包 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

3、全局配置

```
spring:
  application:
    name: cloud-consumer-openfeign-order
  ####Spring Cloud Consul for Service Discovery
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        prefer-ip-address: true #优先使用服务ip进行注册
        service-name: ${spring.application.name}
      circuitbreaker:
        enabled: true
        group:
          enabled: true #没开分组永远不用分组的配置。精确优先、分组次之(开了分组)、默认最后
          
# Resilience4j CircuitBreaker 按照次数：COUNT_BASED 的例子
#  6次访问中当执行方法的失败率达到50%时CircuitBreaker将进入开启OPEN状态(保险丝跳闸断电)拒绝所有请求。
#  等待5秒后，CircuitBreaker 将自动从开启OPEN状态过渡到半开HALF_OPEN状态，允许一些请求通过以测试服务是否恢复正常。
#  如还是异常CircuitBreaker 将重新进入开启OPEN状态；如正常将进入关闭CLOSE闭合状态恢复正常处理请求。
resilience4j:
  circuitbreaker:
    configs:
      default:
        failureRateThreshold: 50 #设置50%的调用失败时打开断路器，超过失败请求百分⽐CircuitBreaker变为OPEN状态。
        slidingWindowType: COUNT_BASED # 滑动窗口的类型
        slidingWindowSize: 6 #滑动窗⼝的⼤⼩配置COUNT_BASED表示6个请求，配置TIME_BASED表示6秒
        minimumNumberOfCalls: 6 #断路器计算失败率或慢调用率之前所需的最小样本(每个滑动窗口周期)。如果minimumNumberOfCalls为10，则必须最少记录10个样本，然后才能计算失败率。如果只记录了9次调用，即使所有9次调用都失败，断路器也不会开启。
        automaticTransitionFromOpenToHalfOpenEnabled: true # 是否启用自动从开启状态过渡到半开状态，默认值为true。如果启用，CircuitBreaker将自动从开启状态过渡到半开状态，并允许一些请求通过以测试服务是否恢复正常
        waitDurationInOpenState: 5s #从OPEN到HALF_OPEN状态需要等待的时间
        permittedNumberOfCallsInHalfOpenState: 2 #半开状态允许的最大请求数，默认值为10。在半开状态下，CircuitBreaker将允许最多permittedNumberOfCallsInHalfOpenState个请求通过，如果其中有任何一个请求失败，CircuitBreaker将重新进入开启状态。
        recordExceptions:
          - java.lang.Exception
    instances:
      cloud-payment-service:
        baseConfig: default          
```

4、写接口

```
@RestController
public class OrderCircuitController
{
    @Resource
    private PayFeignApi payFeignApi;

    @GetMapping(value = "/feign/pay/circuit/{id}")
    @CircuitBreaker(name = "cloud-payment-service", fallbackMethod = "myCircuitFallback")
    public String myCircuitBreaker(@PathVariable("id") Integer id)
    {
        return payFeignApi.myCircuit(id);
    }
    //myCircuitFallback就是服务降级后的兜底处理方法
    public String myCircuitFallback(Integer id,Throwable t) {
        // 这里是容错处理逻辑，返回备用结果
        return "myCircuitFallback，系统繁忙，请稍后再试-----/(ㄒoㄒ)/~~";
    }
}
```

5、测试

当访问正确时，能继续使用

当访问错误率超过阈值时，断路器打开，此时访问正确也不能访问

尝试半打开，成功，则断路器关闭

#### 使用2（基于时间）

##### 介绍

基于时间的滑动窗口是通过有N个桶的环形数组实现。

如果滑动窗口的大小为10秒,这个环形数组总是有10个桶,每个桶统计了在这一秒发生的所有调用的结果(部分统计结果),数组中的第一个桶存储了当前这一秒内的所有调用的结果,其他的桶存储了之前每秒调用的结果。

滑动窗口不会单独存储所有的调用结果,而是对每个桶内的统计结果和总的统计值进行增量的更新,当新的调用结果被记录时,总的统计值会进行增量更新。

检索快照(总的统计值)的时间复杂度为O(1),因为快照已经预先统计好了,并且和滑动窗口大小无关。

关于此方法实现的空间需求(内存消耗)约等于O(n)。由于每次调用结果(元组)不会被单独存储,只是对N个桶进行单独
统计和一次总分的统计。

每个桶在进行部分统计时存在三个整型,为了计算,失败调用数,慢调用数,总调用数。还有一个long类型变量,存储所有
调用的响应时间。

##### 步骤（跟以上步骤相似）

```
server:
  port: 80

spring:
  application:
    name: cloud-consumer-openfeign-order
  ####Spring Cloud Consul for Service Discovery
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        prefer-ip-address: true #优先使用服务ip进行注册
        service-name: ${spring.application.name}
    openfeign:
      client:
        config:
          default:
            #cloud-payment-service:
            #连接超时时间，为避免演示出错，讲解完本次内容后设置为20秒
            connectTimeout: 20000
            #读取超时时间，为避免演示出错，讲解完本次内容后设置为20秒
            readTimeout: 20000
            #开启httpclient5
      httpclient:
        hc5:
          enabled: true
          #开启压缩特性
      compression:
        request:
          enabled: true
          min-request-size: 2048
          mime-types: text/xml,application/xml,application/json
        response:
          enabled: true
      #开启circuitbreaker和分组激活
      circuitbreaker:
        enabled: true
        group:
          enabled: true #没开分组永远不用分组的配置。精确优先、分组次之(开了分组)、默认最后


# feign日志以什么级别监控哪个接口
logging:
  level:
    com:
      atguigu:
        cloud:
          apis:
            PayFeignApi: debug

# Resilience4j CircuitBreaker 按照时间：TIME_BASED 的例子
resilience4j:
  timelimiter:
    configs:
      default:
        timeout-duration: 10s #神坑的位置，timelimiter 默认限制远程1s，超于1s就超时异常，配置了降级，就走降级逻辑
  circuitbreaker:
    configs:
      default:
        failureRateThreshold: 50 #设置50%的调用失败时打开断路器，超过失败请求百分⽐CircuitBreaker变为OPEN状态。
        slowCallDurationThreshold: 2s #慢调用时间阈值，高于这个阈值的视为慢调用并增加慢调用比例。
        slowCallRateThreshold: 30 #慢调用百分比峰值，断路器把调用时间⼤于slowCallDurationThreshold，视为慢调用，当慢调用比例高于阈值，断路器打开，并开启服务降级
        slidingWindowType: TIME_BASED # 滑动窗口的类型
        slidingWindowSize: 2 #滑动窗口的大小配置，配置TIME_BASED表示2秒
        minimumNumberOfCalls: 2 #断路器计算失败率或慢调用率之前所需的最小样本(每个滑动窗口周期)。
        permittedNumberOfCallsInHalfOpenState: 2 #半开状态允许的最大请求数，默认值为10。
        waitDurationInOpenState: 5s #从OPEN到HALF_OPEN状态需要等待的时间
        recordExceptions:
          - java.lang.Exception
    instances:
      cloud-payment-service:
        baseConfig: default
```

### 隔离(舱壁)

#### 简介

隔板来自造船行业，床仓内部一般会分成很多小隔舱，一旦一个隔舱漏水因为隔板的存在而不至于影响其它隔舱和整体船。

类似于此，该功能用于限制并发，限制对于下游服务的最大并发数量的限制

#### 实现方式1（信号量舱壁）

##### 介绍

当信号量有空闲时，进入系统的请求会直接获取信号量并开始业务处理。

当信号量全被占用时，接下来的请求将会进入阻塞状态，SemaphoreBulkhead提供了一个阻塞计时器，

如果阻塞状态的请求在阻塞计时内无法获取到信号量则系统会拒绝这些请求。

若请求在阻塞计时内获取到了信号量，那将直接获取信号量并执行相应的业务处理。

##### 步骤

1、添加接口

提供者

```
@GetMapping(value = "/pay/bulkhead/{id}")
public String myBulkhead(@PathVariable("id") Integer id)
{
    if(id == -4) throw new RuntimeException("----bulkhead id 不能-4");

    if(id == 9999)
    {
        try { TimeUnit.SECONDS.sleep(5); } catch (InterruptedException e) { e.printStackTrace(); }
    }

    return "Hello, bulkhead! inputId:  "+id+" \t " + IdUtil.simpleUUID();
}
```

feign

```
@GetMapping(value = "/pay/bulkhead/{id}")
public String myBulkhead(@PathVariable("id") Integer id);
```

2、添加pom（消费者）

```
<!--resilience4j-bulkhead-->
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-bulkhead</artifactId>
</dependency>
```

3、全局配置

```
####resilience4j bulkhead 的例子
resilience4j:
  bulkhead:
    configs:
      default:
        maxConcurrentCalls: 2 # 隔离允许并发线程执行的最大数量
        maxWaitDuration: 1s # 当达到并发调用数量时，新的线程的阻塞时间，我只愿意等待1秒，过时不候进舱壁兜底fallback
    instances:
      cloud-payment-service:
        baseConfig: default
  timelimiter:
    configs:
      default:
        timeout-duration: 20s
```

4、接口

```
@GetMapping(value = "/feign/pay/bulkhead/{id}")
@Bulkhead(name = "cloud-payment-service",fallbackMethod = "myBulkheadFallback",type = Bulkhead.Type.SEMAPHORE)
public String myBulkhead(@PathVariable("id") Integer id)
{
    return payFeignApi.myBulkhead(id);
}
public String myBulkheadFallback(Throwable t)
{
    return "myBulkheadFallback，隔板超出最大数量限制，系统繁忙，请稍后再试-----/(ㄒoㄒ)/~~";
}
```

5、测试

只有有限线程可以访问

#### 实现方式2（固定线程池舱壁）

##### 简介

FixedThreadPoolBulkhead的功能与SemaphoreBulkhead一样也是用于限制并发执行的次数的，但是二者的实现原理存在差别而且表现效果也存在细微的差别。FixedThreadPoolBulkhead使用一个固定线程池和一个等待队列来实现舱壁。

当线程池中存在空闲时，则此时进入系统的请求将直接进入线程池开启新线程或使用空闲线程来处理请求。

当线程池中无空闲时时，接下来的请求将进入等待队列，

  若等待队列仍然无剩余空间时接下来的请求将直接被拒绝，

  在队列中的请求等待线程池出现空闲时，将进入线程池进行业务处理。

另外：ThreadPoolBulkhead只对CompletableFuture方法有效，所以我们必创建返回CompletableFuture类型的方法

##### 步骤

1、添加依赖

```
<!--resilience4j-bulkhead-->
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-bulkhead</artifactId>
</dependency>
```

2、设置配置

```
server:
  port: 80

spring:
  application:
    name: cloud-consumer-openfeign-order
  ####Spring Cloud Consul for Service Discovery
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        prefer-ip-address: true #优先使用服务ip进行注册
        service-name: ${spring.application.name}
    openfeign:
      client:
        config:
          default:
            #cloud-payment-service:
            #连接超时时间，为避免演示出错，讲解完本次内容后设置为20秒
            connectTimeout: 20000
            #读取超时时间，为避免演示出错，讲解完本次内容后设置为20秒
            readTimeout: 20000
            #开启httpclient5
      httpclient:
        hc5:
          enabled: true
          #开启压缩特性
      compression:
        request:
          enabled: true
          min-request-size: 2048
          mime-types: text/xml,application/xml,application/json
        response:
          enabled: true
      #开启circuitbreaker和分组激活
      circuitbreaker:
        enabled: true
#        group:
#          enabled: true # 演示Bulkhead.Type.THREADPOOL时spring.cloud.openfeign.circuitbreaker.group.enabled设为false新启线程和原来主线程脱离了。



# feign日志以什么级别监控哪个接口
logging:
  level:
    com:
      atguigu:
        cloud:
          apis:
            PayFeignApi: debug

####resilience4j bulkhead -THREADPOOL的例子
resilience4j:
  timelimiter:
    configs:
      default:
        timeout-duration: 10s #timelimiter默认限制远程1s，超过报错不好演示效果所以加上10秒
  thread-pool-bulkhead:
    configs:
      default:
        core-thread-pool-size: 1
        max-thread-pool-size: 1
        queue-capacity: 1
    instances:
      cloud-payment-service:
        baseConfig: default
# spring.cloud.openfeign.circuitbreaker.group.enabled 请设置为false 新启线程和原来主线程脱离
```

3、实现接口

```
@GetMapping(value = "/feign/pay/bulkhead/{id}")
@Bulkhead(name = "cloud-payment-service",fallbackMethod = "myBulkheadPoolFallback",type = Bulkhead.Type.THREADPOOL)
public CompletableFuture<String> myBulkheadTHREADPOOL(@PathVariable("id") Integer id)
{
    System.out.println(Thread.currentThread().getName()+"\t"+"enter the method!!!");
    try { TimeUnit.SECONDS.sleep(3); } catch (InterruptedException e) { e.printStackTrace(); }
    System.out.println(Thread.currentThread().getName()+"\t"+"exist the method!!!");

    return CompletableFuture.supplyAsync(() -> payFeignApi.myBulkhead(id) + "\t" + " Bulkhead.Type.THREADPOOL");
}
public CompletableFuture<String> myBulkheadPoolFallback(Integer id,Throwable t)
{
    return CompletableFuture.supplyAsync(() -> "Bulkhead.Type.THREADPOOL，系统繁忙，请稍后再试-----/(ㄒoㄒ)/~~");
}
```

##### 参数配置

##### 接口详解

| 配置属性           | 默认值                                          | 描述                                                         |
| :----------------- | :---------------------------------------------- | :----------------------------------------------------------- |
| 最大线程池大小     | Runtime.getRuntime() .availableProcessors()     | 配置最大线程池大小。                                         |
| 核心线程池大小     | Runtime.getRuntime() .availableProcessors() - 1 | 配置核心线程池大小                                           |
| 队列容量           | 100                                             | 配置队列的容量。                                             |
| 保持活动持续时间   | 20 [毫秒]                                       | 当线程数大于核心数时，这是多余的空闲线程在终止之前等待新任务的最长时间。 |
| 可写StackTrace启用 | 真的                                            | 当引发舱壁异常时输出堆栈跟踪错误。 如果为 false，则输出带有舱壁异常的单行。 |

### 限流

#### 简介

限流就是限制最大访问流量。系统能提供的最大并发是有限的，同时来的请求又太多，就需要限流。 

比如商城秒杀业务，瞬时大量请求涌入，服务器忙不过就只好排队限流了，和去景点排队买票和去医院办理业务排队等号道理相同。

所谓限流，就是通过对并发访问/请求进行限速，或者对一个时间窗口内的请求进行限速，以保护应用系统，一旦达到限制速率则可以拒绝服务、排队或等待、降级等处理。

#### 算法

##### 漏桶算法 

一个固定容量的漏桶，按照设定常量固定速率流出水滴，类似医院打吊针，不管你源头流量多大，我设定匀速流出。 

如果流入水滴超出了桶的容量，则流入的水滴将会溢出了(被丢弃)，而漏桶容量是不变的。

![image-20240324161119571](.\CircuitBreaker\image-20240324161119571.png)

这里有两个变量，一个是桶的大小，支持流量突发增多时可以存多少的水（burst），另一个是水桶漏洞的大小（rate）。因为漏桶的漏出速率是固定的参数，所以，即使网络中不存在资源冲突（没有发生拥塞），漏桶算法也不能使流突发（burst）到端口速率。因此，漏桶算法对于存在突发特性的流量来说缺乏效率。

##### 令牌桶算法（springcloud默认）

令牌桶算法是程序以r（r=时间周期/限流值）的速度向令牌桶中增加令牌，直到令牌桶满，请求到达时向令牌桶请求令牌，如获取到令牌则通过请求，否则触发限流策略

![](.\CircuitBreaker\20190716090944463.png)

##### 滚动时间窗(tumbling time window)

允许固定数量的请求进入(比如1秒取4个数据相加，超过25值就over)超过数量就拒绝或者排队，等下一个时间段进入。

由于是在一个时间间隔内进行限制，如果用户在上个时间间隔结束前请求（但没有超过限制），同时在当前时间间隔刚开始请求（同样没超过限制），在各自的时间间隔内，这些请求都是正常的。下图统计了3次，but......

![image-20240324162325849](.\CircuitBreaker\image-20240324162325849.png)

##### 滑动时间窗口（sliding time window）

顾名思义，该时间窗口是滑动的。所以，从概念上讲，这里有两个方面的概念需要理解： 

\- 窗口：需要定义窗口的大小

\- 滑动：需要定义在窗口中滑动的大小，但理论上讲滑动的大小不能超过窗口大小

滑动窗口算法是把固定时间片进行划分并且随着时间移动，移动方式为开始时间点变为时间列表中的第2个时间点，结束时间点增加一个时间点，不断重复，通过这种方式可以巧妙的避开计数器的临界点的问题。下图统计了5次

![image-20240324162630789](.\CircuitBreaker\image-20240324162630789.png)

#### 步骤

1、提供者创建接口，feign创建方法

```
//=========Resilience4j ratelimit 的例子
@GetMapping(value = "/pay/ratelimit/{id}")
public String myRatelimit(@PathVariable("id") Integer id)
{
    return "Hello, myRatelimit欢迎到来 inputId:  "+id+" \t " + IdUtil.simpleUUID();
}
```

```
@GetMapping(value = "/pay/ratelimit/{id}")
public String myRatelimit(@PathVariable("id") Integer id);
```

2、添加依赖

```
<!--resilience4j-ratelimiter-->
<dependency>
    <groupId>io.github.resilience4j</groupId>
    <artifactId>resilience4j-ratelimiter</artifactId>
</dependency>
```

3、配置文件

```
server:
  port: 80

spring:
  application:
    name: cloud-consumer-openfeign-order
  ####Spring Cloud Consul for Service Discovery
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        prefer-ip-address: true #优先使用服务ip进行注册
        service-name: ${spring.application.name}
    openfeign:
      client:
        config:
          default:
            #cloud-payment-service:
            #连接超时时间，为避免演示出错，讲解完本次内容后设置为20秒
            connectTimeout: 20000
            #读取超时时间，为避免演示出错，讲解完本次内容后设置为20秒
            readTimeout: 20000
            #开启httpclient5
      httpclient:
        hc5:
          enabled: true
          #开启压缩特性
      compression:
        request:
          enabled: true
          min-request-size: 2048
          mime-types: text/xml,application/xml,application/json
        response:
          enabled: true
      #开启circuitbreaker和分组激活
      circuitbreaker:
        enabled: true
#        group:
#          enabled: true # 演示Bulkhead.Type.THREADPOOL时spring.cloud.openfeign.circuitbreaker.group.enabled设为false新启线程和原来主线程脱离了。



# feign日志以什么级别监控哪个接口
logging:
  level:
    com:
      atguigu:
        cloud:
          apis:
            PayFeignApi: debug

####resilience4j ratelimiter 限流的例子
resilience4j:
  ratelimiter:
    configs:
      default:
        limitForPeriod: 2 #在一次刷新周期内，允许执行的最大请求数
        limitRefreshPeriod: 1s # 限流器每隔limitRefreshPeriod刷新一次，将允许处理的最大请求数量重置为limitForPeriod
        timeout-duration: 1 # 线程等待权限的默认等待时间
    instances:
      cloud-payment-service:
        baseConfig: default
```

4、调用方法，创建接口

```
@GetMapping(value = "/feign/pay/ratelimit/{id}")
@RateLimiter(name = "cloud-payment-service",fallbackMethod = "myRatelimitFallback")
public String myBulkhead(@PathVariable("id") Integer id)
{
    return payFeignApi.myRatelimit(id);
}
public String myRatelimitFallback(Integer id,Throwable t)
{
    return "你被限流了，禁止访问/(ㄒoㄒ)/~~";
}
```