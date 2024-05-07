- 创建并运行项目

  - 配置jDK（springboot2：jdk最低为8，spring最低5；springboot3：jdk最低为17，spring最低为6）

  - 配置Maven（[1、Maven](https://mubu.com/docV-HA0TtDA0) ）

- IDEA集成springboot

  - 新建项目

    - Spring Initializer

      

      - Server URL：获取项目模板和依赖信息，然后生成项目的基础结构

        - jdk8、11、17：[https://start.aliyun.com](https://start.aliyun.com/)

        - jdk17、21：[https://start.spring.io](https://start.spring.io/)

  - 导入项目

- 注解：

  - 总结：

    - [肝了一周总结的SpringBoot常用注解大全，一目了然！ - 掘金 (juejin.cn)](https://juejin.cn/post/7179038534711967800#comment)

    - [SpringBoot最全注解大全-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1921919)

- 依赖（pom.xml）

  - pom文件：https://blog.csdn.net/qq_41978323/article/details/122381265

  - lombook：自动生成构造器、getter/setter、equals、hashcode、toString等方法，自动化生成日志变量

    - 特点：Lombok会在编译时，自动生成对应的java代码。我们使用lombok时，还需要安装一个lombok的插件(idea自带)

    - 依赖![img](https://api2.mubu.com/v3/document_image/5060ca5a-3e0f-43be-8747-d6d8c7d96851-18846868.jpg)
      <dependency>
      ​   <groupId>org.projectlombok</groupId>
      ​   <artifactId>lombok</artifactId
      ​</dependency>

    - 注解

      

      

      - @ data![img](https://api2.mubu.com/v3/document_image/bc79a713-1c68-4572-8e39-52e93f86845c-18846868.jpg)

  - Slf4j：日志管理

    - 依赖
      <dependency>
      ​<groupId>org.slf4j</groupId>
      ​<artifactId>slf4j-log4j12</artifactId>
      ​<version>2.0.7</version>
      ​</dependency>

    - 使用：
      - 

-  配置

  - 热部署：（项目运行后，修改代码后无需再次运行就可以使用修改后内容）

    - 1、依赖

    - 2、自动构建项目![img](https://api2.mubu.com/v3/document_image/462034f9-dab6-483c-9a48-323ea6cd5c8f-18846868.jpg)

    - 3、自动make启动![img](https://api2.mubu.com/v3/document_image/8d827cf4-842e-4423-a4af-17c36210a612-18846868.jpg)

  - 项目全局配置

    - 全局配置文件

      - 全局配置文件：application(properties/yml)

        - 文件类型(properties/yml)

          - properties：一行一个，使用 . 分开

          - yml：换行，用空格分开，冒号后面也要有空格

          - 文件转换![img](https://api2.mubu.com/v3/document_image/66f20574-d455-4019-9f31-76e3e59765e9-18846868.jpg)

        - 配置文件优先级：https://blog.csdn.net/ai97926/article/details/127618284

        - 文件内容(key value)

          - 为接口添加根路径：server.servlet.context-path=/根路径名

          - 修改端口号：server.port=端口号

      - 多环境配置：不同的环境(开发/测试/生产)使用不同的环境配置

        - 文件配置

          - 文件格式：application-[profile].properties![img](https://api2.mubu.com/v3/document_image/d3ef4baf-7455-4996-927f-e4bf1977b943-18846868.jpg)

          - 激活环境配置

            - 方式1：控制台执行：java -jar xxx.jar --1=1dev

            - 方式2：全局配置文件：spring.profiles.active=dev

        - 类配置

          - 在类上使用@ Profile 注解

          - 案例：根据环境不同，生成不同的bean对象

            - 1、接口![img](https://api2.mubu.com/v3/document_image/74fd39de-d0a0-4064-866a-f575e17e63cc-18846868.jpg)

            - 2、实现类

              

              - DevDBConnector![img](https://api2.mubu.com/v3/document_image/2ac55aa9-f8f3-4e73-99f8-478fa30a533e-18846868.jpg)

              - TestDBConnector![img](https://api2.mubu.com/v3/document_image/ef3e9030-4e12-4e94-b60a-1311c9117ea7-18846868.jpg)

              - ProdDBConnector ![img](https://api2.mubu.com/v3/document_image/1763bba8-9a85-4bd3-94ab-3d80107949b1-18846868.jpg)

            - 3、调用![img](https://api2.mubu.com/v3/document_image/79ee46cc-b78d-4345-8d90-250a1965340c-18846868.jpg)

            - 4、激活某个环境![img](https://api2.mubu.com/v3/document_image/1441f536-dfd7-45f3-b2b1-990dd0f04e10-18846868.jpg)

    - 系统配置： java -Dxxx=xxx

    - 命令行参数配置：java --xxx=xxx 
      - 运行jar包![img](https://api2.mubu.com/v3/document_image/88987ec7-9ac7-4d47-b22f-8871401b60de-18846868.jpg)

  - 配置文件注入属性值

    - 加载配置文件
      - 1、全局配置文件(properties)默认加载

    - 配置文件注入类属性值

      - @ ConfigurationProperties 注入属性（可以多级属性获取）![img](https://api2.mubu.com/v3/document_image/3a7db387-4970-4253-8d43-4be4bcd3b0fc-18846868.jpg)

      - @ Value 注入属性![img](https://api2.mubu.com/v3/document_image/659c5504-734d-4d64-9cd0-c980c9718010-18846868.jpg)

      - 对比

        

        - 如果只是针对某一个业务要求，要引入配置文件的个别属性值，推荐使用@Value注解 

        - 如果针对某个JavaBean类，需要注入批量的属性值，则推荐用@ConfigurationProperties注解

- 核心内容

  - IOC/DI

    - bean对象创建（控制反转）

      - 直接使用注解

        - @Component: 作为通用的组件注解，标识一个类为Spring容器中的一个Bean对象。

        - @Controller: 用于标识控制层组件，通常用于处理HTTP请求和响应。

        - @Service: 用于标识服务层组件，通常用于封装业务逻辑。

        - @ Mapper:用于持久层数据访问

        - @Repository: 用于标识数据访问层组件，通常用于访问数据库或其他持久化操作。

        - @RestController: 等同于@Controller和@ResponseBody的组合注解，用于标识RESTful风格的控制层组件。

      - @ Configuration + @ Bean

        - 步骤：

          

          - 1、创建类添加 @ Configuration 

          - 2、方法中添加@ Bean，new对象并返回对象

      - @ ComponentScan 组件扫描（bean对象不在启动类同路径下时使用）
        - 语法![img](https://api2.mubu.com/v3/document_image/5dc0296f-0285-4d07-961b-22643fcddc6a-18846868.jpg)

      - @ Import导入类

        - 普通类导入

        - 配置类导入 

        - ImportSelector 接口实现类导入

      - 获取其他项目bean对象

      - 配置bean对象为其他项目使用

    - bean对象配置

      - @ Scope 配置作用实例数量

        - 语法![img](https://api2.mubu.com/v3/document_image/ce2cea8a-9823-4264-a3cb-5f54162c889f-18846868.jpg)

        - 案例![img](https://api2.mubu.com/v3/document_image/16ffd5ea-14c0-4c04-96c2-e50cc3df89d7-18846868.jpg)

    - bean对象调用（依赖注入）

      - Autowired

        - 原理：

          - 自动装配：@Autowired注解标记在需要进行依赖注入的字段、构造方法或者Setter方法上，告诉Spring容器自动装配相应的依赖对象。Spring容器会在初始化Bean时扫描带有@Autowired注解的成员，并尝试为其注入合适的依赖对象。

          - 依赖查找：当Spring容器遇到标记了@Autowired注解的字段、构造方法或者Setter方法时，它会根据类型匹配原则去查找合适的依赖对象。Spring会在容器中查找与被注入字段类型或参数类型匹配的Bean对象。

          - 自动连接：当Spring容器找到匹配的依赖对象后，会将其自动连接到目标Bean中。具体连接方式取决于被注入字段的类型和访问控制（如私有字段、公有字段、Setter方法等）。

          - 解决歧义性：当存在多个符合条件的候选对象时，Spring会根据一定的规则解析并选择合适的依赖对象。可以使用@Qualifier注解来进一步指定具体的Bean对象。

          - 循环依赖处理：Spring通过使用Bean的代理对象来解决循环依赖问题。在初始化Bean时，如果出现循环依赖，Spring会提前暴露一个尚未完全初始化的代理对象，以满足依赖关系的建立。

        - 注入方式：

          - 属性字段注入![img](https://api2.mubu.com/v3/document_image/036e539b-e044-4dba-97ba-003ea8bb7268-18846868.jpg)

          - 构造方法注入![img](https://api2.mubu.com/v3/document_image/43091894-93be-43b0-923a-93a3a555dd27-18846868.jpg)

          - setter方法注入![img](https://api2.mubu.com/v3/document_image/889ac6cf-baba-4510-8490-c2d7627513a8-18846868.jpg)

        - 非必要注入：required=false（如果没有找到匹配的Bean，Spring容器不会抛出异常，而是设置为null）![img](https://api2.mubu.com/v3/document_image/60fec847-089b-4e6e-b890-a0a273eaef8c-18846868.jpg)

        - Qualifier注解：按照名称进行装配![img](https://api2.mubu.com/v3/document_image/74f96352-e6b5-4a5c-a771-7c50cb2ea986-18846868.jpg)

      - Autowired、Resource、Inject之间区别

        - 来源和兼容性：

          - @Autowired 是 Spring 框架提供的注解，用于进行依赖注入。它是 Spring 特有的注解，可以与 Spring 的其他功能（如自动装配、AOP等）无缝集成。

          - @Resource 是 Java EE 提供的注解，用于进行依赖注入或资源注入。它是 Java EE 规范定义的一部分，可以在非 Spring 环境下使用，并且与 Spring 兼容。

          - @Inject 是 Java 的依赖注入规范（JSR-330）中定义的注解，可以在支持 JSR-330 的依赖注入容器中使用。与 @Autowired 和 @Resource 类似，但不局限于 Spring 环境。

        - 注入方式：

          - @Autowired 可以标注在字段、setter 方法、构造方法或参数上，通过类型匹配来完成自动注入。

          - @Resource 可以标注在字段、setter 方法或构造方法上，通过名称匹配来完成注入。如果没有指定名称，默认按照字段名或方法名进行匹配。

          - @Inject 可以标注在字段、setter 方法、构造方法或参数上，通过类型匹配来完成注入。

        - 属性可选性：

          - @Autowired 默认要求注入的依赖项必须存在，如果找不到匹配的依赖项，会抛出异常。可以通过设置 required = false 来将依赖项设为可选。

          - @Resource 默认要求注入的依赖项必须存在，如果找不到匹配的依赖项，会抛出异常。可以通过设置 optional = true 来将依赖项设为可选。

          - @Inject 默认要求注入的依赖项必须存在，如果找不到匹配的依赖项，会抛出异常。没有类似的属性来设置可选性。

        - 来源选择：

          - @Autowired 和 @Inject 都是通过类型匹配来完成注入，可以与 Spring 的自动装配功能结合使用。如果存在多个匹配的依赖项，可以通过限定符（@Qualifier）或主要性（@Primary）进行进一步的选择。

          - @Resource 主要通过名称匹配来完成注入，可以指定要注入的名称或 ID。如果没有指定名称，默认按照字段名或方法名进行匹配。

  - AOP（面向切面编程）

    - 基础案例：统计每一个业务方法的执行耗时

      - 1、添加依赖![img](https://api2.mubu.com/v3/document_image/889ce3fd-3d45-46fb-a85a-da252548a540-18846868.jpg)
        <dependency>
        ​    <groupId>org.springframework.boot</groupId>
        ​    <artifactId>spring-boot-starter-aop</artifactId>
        ​</dependency>

      - 2、AOP类![img](https://api2.mubu.com/v3/document_image/d7ab46ca-a8db-434c-8e58-d49a398b8704-18846868.jpg)

    - 基础语法：

      

      - 连接点：JoinPoint，可以被AOP控制的方法（暗含方法执行时的相关信息）

      - 通知：Advice，指哪些重复的逻辑，也就是共性功能（最终体现为一个方法）

      - 切入点：PointCut，匹配连接点的条件，通知仅会在切入点方法执行时被应用

      - 切面：Aspect，描述通知与切入点的对应关系（通知+切入点）

      - 目标对象：Target，通知所应用的对象

    - 通知

      - 类型  

        - @ Around（环绕通知）

        - @ Before（前置通知）
          - 在调用ProceedingJoinPoint.proceed()前启动，所以@ Around前部分内容先执行

        - @ After（后置通知）
          - 在调用ProceedingJoinPoint.proceed()后启动，在@ Around后部分内容前执行

        - @ AfterReturning（返回后通知）

        - @ AfterThrowing（异常后通知）

      - 细节

        - @ Around环绕通知需要自己调用 ProceedingJoinPoint.proceed() 来让原始方法执行，其他通知不需要考虑目标方法执行

        - @ Around环绕通知方法的返回值，必须指定为Object，来接收原始方法的返回值。

      - 顺序

        - 不同切面类中，默认按照切面类的类名字母排序：

          - 目标方法前的通知方法：字母排名靠前的先执行

          - 目标方法后的通知方法：字母排名靠前的后执行

        - 使用  @ Order 注解

          - 目标方法前的通知方法：数字小的先执行

          - 目标方法前的通知方法：数字大的先执行

    - 切入点表达式语法：

      - execution(……)：根据方法的签名来匹配

        

        - 带问号可以省略

          - 访问修饰符：可省略（比如: public、protected）

          - 包名.类名： 可省略

          - throws 异常：可省略（注意是方法上声明抛出的异常，不是实际抛出的异常）

        - \* ：单个独立的任意符号，可以通配任意返回值、包名、类名、方法名、任意类型的一个参数，也可以通配包、类、方法名的一部分![img](https://api2.mubu.com/v3/document_image/4bda0254-0c23-450a-8408-ee93dee6c33f-18846868.jpg)

        - .. ：多个连续的任意符号，可以通配任意层级的包，或任意类型、任意个数的参数![img](https://api2.mubu.com/v3/document_image/8f12d1c3-15d0-4961-a3ee-9661eb364e09-18846868.jpg)

        - 案例![img](https://api2.mubu.com/v3/document_image/cfc42ebc-98ae-40ef-8bfd-ae7e1da58720-18846868.jpg)

      - @annotation(……) ：根据注解匹配（根据是否调用了相应的注解）

        - 步骤1：定义一个注解![img](https://api2.mubu.com/v3/document_image/5b1a9e3d-f5b0-4409-85d5-c214f6464402-18846868.jpg)

        - 步骤2：@ annotation注解表示凡是使用了MyLog注解的方法都会调用该方法![img](https://api2.mubu.com/v3/document_image/0173f102-d69c-4ff0-92ec-956d83fdef2e-18846868.jpg)

        - 步骤3：添加注解![img](https://api2.mubu.com/v3/document_image/2cdcb002-dce6-4ef8-9939-a3235f59ee98-18846868.jpg)

    - 重复切入点表达式：将表达式转为方法，然后重复调用使用
      - 语法![img](https://api2.mubu.com/v3/document_image/e169ac38-63fa-4db1-b4c4-95c7914f336f-18846868.jpg)

    - 连接点

      - 对于 @Around 通知，获取连接点信息只能使用  ProceedingJoinPoint![img](https://api2.mubu.com/v3/document_image/da2c4078-f423-4972-b416-e663e073c202-18846868.jpg)

      - 对于其他四种通知，获取连接点信息只能使用 JoinPoint ，它是 ProceedingJoinPoint 的父类型![img](https://api2.mubu.com/v3/document_image/c28264d5-72cb-407e-9600-7914a8f605c1-18846868.jpg)

  - 事务

  - 文件资源

- 项目框架

  - java

    - controller（接口层）（[3.1、控制层](https://mubu.com/doc6TWOxBgN9g0) ）

    - service（业务层）（[3.2、服务层](https://mubu.com/docNxrSYGsly0) ）

    - mapper（持久层）（[3.3、持久层](https://mubu.com/doc6eXMVUEfCKQ)  ）

    - entity：实体类

    - config：配置类

    - aop：面向切面编程

  - resources

    - statics：静态资源

    - template：模板/动态资源（[Thymeleaf](https://fanlychie.github.io/post/thymeleaf.html)）

    - application.properties/yml/yaml：全局配置文件