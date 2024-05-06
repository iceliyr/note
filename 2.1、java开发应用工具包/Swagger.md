## Swagger

* Swagger是一种基于OpenAPI规范的API文档生成工具，它可以根据Java代码中的注解自动生成API接口文档，并提供UI界面进行在线测试和调试。

* Swagger为开发人员提供了更加方便、直观的API管理方式，有助于提升API的可读性和可维护性。

* Swagger的主要特点包括：

1、自动生成API文档：通过在Java代码中添加Swagger注解，Swagger能够自动地解析API接口的参数、响应等信息，并生成相应的API文档。

2、在线测试接口：Swagger提供了UI界面，可以方便地进行API接口的测试和调试，无需单独使用HTTP客户端来测试接口。

3、支持多种语言和框架：Swagger不仅支持Java语言和Spring框架，还支持多种其他语言和框架，如PHP、Python、Go等。

4、扩展性强：Swagger提供了多种扩展机制和插件，可以满足各种项目的需要，如集成OAuth2、自定义UI等。

Swagger提供的UI界面相比于另外一款Api文档生成工具**Knife4j**较为简陋。



## Knife4j

#### Knife4j简介

官方文档：https://doc.xiaominfo.com/

* Knife4j是一种基于Swagger构建的增强工具，它在Swagger的基础上增加了更多的功能和扩展，提供了更加丰富的API文档管理功能。相比于原版Swagger，Knife4j的主要特点包括：

1、更加美观的UI界面：Knife4j通过对Swagger UI的修改和优化，实现了更加美观、易用的UI界面，提升了开发人员的体验感。

2、支持多种注解配置方式：除了支持原版Swagger的注解配置方式外，Knife4j还提供了其他几种注解配置方式，方便开发人员进行不同场景下的配置。

3、提供多种插件扩展：Knife4j提供了多种插件扩展，如knife4j-auth、knife4j-rate-limiter等，可以满足不同项目的需求。

4、集成Spring Boot Starter：Knife4j发布了spring-boot-starter-knife4j，可以实现更加便捷的集成，并支持配置文件中的动态属性调整。

#### Knife4j使用

官方文档使用地址：https://doc.xiaominfo.com/docs/quick-start

具体的步骤：

在common-service模块中添加knife4j所需要的配置类

```java
@Configuration
public class Knife4jConfig {

    @Bean
    public GroupedOpenApi adminApi() {      // 创建了一个api接口的分组
        return GroupedOpenApi.builder()
                .group("admin-api")         // 分组名称
                .pathsToMatch("/admin/**")  // 接口请求路径规则
                .build();
    }

    /***
     * @description 自定义接口信息
     */
    @Bean
    public OpenAPI customOpenAPI() {

        return new OpenAPI()
                 .info(new Info()
                 .title("尚品甑选API接口文档")
                 .version("1.0")
                 .description("尚品甑选API接口文档")
                 .contact(new Contact().name("atguigu"))); // 设定作者
    }

}
```

启动项目就可以访问到knife4j所生成的接口文档了。访问地址：**http://项目ip:端口号/doc.html**

![image-20230509194447547](./C:/Users/李艺儒/Desktop/微服务项目/尚品甄选项目课件/assets/image-20230509194447547.png) 

#### 常见注解

在Knife4j中也提供了一些注解，让我们对接口加以说明，常见的注解如下所示：

```shell
@Tag： 用在controller类上，对controller进行说明
@Operation: 用在controller接口方法上对接口进行描述
@Parameters：用在controller接口方法上对单个参数进行描述
@Schema： 用在实体类和实体类属性上，对实体类以及实体类属性进行描述
```

举例说明：

```java
@Tag(name = "首页接口")
public class IndexController {


    @Operation(summary = "用户登录")
    public Result<LoginVo> login(@RequestBody LoginDto loginDto) {
        ...
    }

    @Operation(summary = "用户退出")
    @Parameters(value = {
            @Parameter(name = "令牌参数" , required = true)
    })
    @GetMapping(value = "/logout")
    public Result logout(@RequestHeader(value = "token") String token) {
        ...
    }

}


@Data
@Schema(description = "用户登录请求参数")
public class LoginDto {

    @Schema(description = "用户名")
    private String userName ;

    @Schema(description = "密码")
    private String password ;

    @Schema(description = "提交验证码")
    private String captcha ;

    @Schema(description = "验证码key")
    private String codeKey ;

}
```

#### 导出

Knife4j生成的接口文档是一个在线的接口文档使用起来不是特别的方便，当然Knife4j也支持离线接口文档，并且支持导出json格式的数据，如下所示：
