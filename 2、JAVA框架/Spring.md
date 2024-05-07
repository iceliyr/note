- Spring体系：https://mikechen.cc/30127.html

  - SpringFramework![img](https://api2.mubu.com/v3/document_image/cd122d9b-4754-42e6-9f78-b316793a2c86-18846868.jpg)

  - SpringMVC![img](https://api2.mubu.com/v3/document_image/e392c80c-8f7a-4de5-b9fc-c5f917fe2827-18846868.jpg)

  - SpringBoot

    - 起步依赖

    - 自动配置

  - SpringCloud![img](https://api2.mubu.com/v3/document_image/b4319c76-a216-4435-b1ef-9072cc3666c3-18846868.jpg)

- spring笔记：https://www.wolai.com/oacbJpH1wPzGNoMAVnoELR

- SpringFramework

  - IOC/DI

    - IOC/DI原理

      - IOC容器分类
        - 继承关系图![img](https://api2.mubu.com/v3/document_image/a525caca-5059-4b1e-b6f0-af3644ad7cf3-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/cc68abb3-1d47-4502-a571-7cd93c013ce2-18846868.jpg)

      - IOC容器（创建和管理bean对象）
        - xml![img](https://api2.mubu.com/v3/document_image/8d61f708-f341-436c-bc8d-8767b08e089c-18846868.jpg)

      - IOC（控制反转）
        - IoC 容器维护着构成应用程序的对象，并负责创建这些对象

      - DI（依赖注入）

        - 方式：XML 配置文件、注解

        - 形式：构造函数注入、Setter 方法注入和接口注入

    - IOC/DI应用

      - springframework：

        - xml文件

          - 步骤

            - 0、创建组件类

            - 1、创建配置文件

              - 配置bean对象（控制反转）

                - 无参构造![img](https://api2.mubu.com/v3/document_image/982580ca-f8ce-42ed-b152-3d82e2f185fd-18846868.jpg)

                - 静态工厂方法（方法返回值必须是相同类或者是子类）![img](https://api2.mubu.com/v3/document_image/d0c4c61e-7047-41ee-b218-86b88272b2da-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/4bcde2a2-f2e0-46e2-9b64-f9523b280676-18846868.jpg)

                - 实例工厂方法（创建新bean时，在已有的bean对象上调用非静态方法）![img](https://api2.mubu.com/v3/document_image/fd8e1cf4-5758-4e6a-9327-0afaddbdb48e-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/a3b54ddb-78a6-4d92-8e8d-c57ce75184f0-18846868.jpg)

              - 依赖注入

                - bean对象类型（依赖注入）![img](https://api2.mubu.com/v3/document_image/b772220b-6bc9-4762-92fe-8f7c48b9390f-18846868.jpg)

                - 构造器（依赖注入）![img](https://api2.mubu.com/v3/document_image/1e648059-4b02-4f18-9c11-673970564bc9-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/d0465c19-2914-45b8-95e9-e19a7509d9c6-18846868.jpg)

                - setter方法![img](https://api2.mubu.com/v3/document_image/4533d87f-dcd4-4653-afd9-b1d9439a11fd-18846868.jpg)

              - 配置类属性值（<property  name="属性名"> </property>）

                - 基本类型![img](https://api2.mubu.com/v3/document_image/358e4e5c-fa5e-4707-8c0d-2e45431c3ba8-18846868.jpg)

                - 字符串类型![img](https://api2.mubu.com/v3/document_image/337d6733-663d-4a0d-871b-7bc56a5319de-18846868.jpg)

                - 集合类型（list、set、array）![img](https://api2.mubu.com/v3/document_image/172f7343-cd29-4da9-810c-b6903b40419a-18846868.jpg)

                - key-value类型![img](https://api2.mubu.com/v3/document_image/621c015c-54d4-497c-b7e6-445aa8d3faea-18846868.jpg)

            - 2、实例化容器（创建容器）![img](https://api2.mubu.com/v3/document_image/0d27189e-2e87-4272-9cad-332c6af8782d-18846868.jpg)

            - 3、依赖注入

              - 方式1：依赖注入

              - 方式2：通过具体信息读取![img](https://api2.mubu.com/v3/document_image/28b71b1a-acac-44a2-b8cc-bf2316423a06-18846868.jpg)

          - bean特性

            - bean周期方法（初始化和释放资源）![img](https://api2.mubu.com/v3/document_image/62e990a9-a389-4acf-9879-925887bac8c3-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/79824794-f726-4cf6-8457-1c4a966ab6ea-18846868.jpg)

            - bean作用域![img](https://api2.mubu.com/v3/document_image/73486be0-d509-431f-b348-061a2248d6a4-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/b6e32305-9df4-4f9d-ac1d-e68ff5b8e92e-18846868.jpg)

          - FactoryBean

            - 作用：可以自定义任何所需的初始化逻辑，生产出一些定制化的 bean

            - 使用：

              - 1、实现接口，实现方法

              - 2、配置信息

            - 高级：[FactoryBean](https://zhuanlan.zhihu.com/p/229003633)

          - XML文件

            - 引入文件：<context:property-placeholder location="classpath:jdbc.properties" />

            - 引入配置文件属性值：${xxx.xxx.xxx}

        - 注解+XML

          - 注解：[spring中需要掌握的25个常用注解](https://juejin.cn/post/7001843959731339271)

          - 步骤：

            - 1、依赖

            - 2、添加注解

              - 控制反转

                - 添加id![img](https://api2.mubu.com/v3/document_image/763132e7-6f85-408f-8a84-86ca15fd06d1-18846868.jpg)

                - 周期函数![img](https://api2.mubu.com/v3/document_image/2c9fbde4-712b-4ae6-b48c-ccf020bf34be-18846868.jpg)

                - 作用域：![img](https://api2.mubu.com/v3/document_image/18ac1af4-e3de-4961-b8d1-f91d63d2d860-18846868.jpg)

              - 依赖注入

                - Autowired

                  - 注解成员

                    - 属性

                    - 构造器

                    - set方法

                  - 原理![img](https://api2.mubu.com/v3/document_image/ed99d0b8-6eb8-4b9f-a0e7-7f8afdc68b1c-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/7590a8aa-fea9-4e81-9e13-084e3e76c7db-18846868.jpg)

                - Resource![img](https://api2.mubu.com/v3/document_image/49e45145-0391-467b-a849-54317bd90c4f-18846868.jpg)

              - 配置属性值

                - 1、配置文件![img](https://api2.mubu.com/v3/document_image/3e6b51f4-3842-4e0d-9dd5-929ecbe282b0-18846868.jpg)

                - 2、Value注解引入![img](https://api2.mubu.com/v3/document_image/91eb6fc6-9c39-4ffa-a14e-ba8df88dc140-18846868.jpg)

            - 3、配置文件（XML）

              - bean对象

                - 1、指定扫描范围![img](https://api2.mubu.com/v3/document_image/e7923453-b1e6-4079-8206-2961234f8089-18846868.jpg)

                - 2、指定排除组件![img](https://api2.mubu.com/v3/document_image/ca4d5530-c023-4cd9-9394-c6f8003ec1f6-18846868.jpg)

                - 3、指定扫描组件![img](https://api2.mubu.com/v3/document_image/19d6f339-249d-46ae-b18b-1c095f10db40-18846868.jpg)

              - 属性值
                - 引入文件![img](https://api2.mubu.com/v3/document_image/8281f416-2873-4b7d-b79e-c714339c9c97-18846868.jpg)

            - 4、应用配置文件

            - 5、依赖注入

          - 归纳：

            - 1. 注解负责标记IoC的类和进行属性装配

              

            - 2. xml文件依然需要，需要通过

              

            - 3. 标记IoC注解：@Component,@Service,@Controller,@Repository

              

            - 4. 标记DI注解：@Autowired @Qualifier @Resource @Value

              

            - 5. IoC具体容器实现选择ClassPathXmlApplicationContext对象

              

        - 注解+配置类

          - 配置类定义、组件扫描范围、配置属性值：![img](https://api2.mubu.com/v3/document_image/b3f02be4-4df4-4cb6-bb1b-a858f6b8c9b0-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/9e7a5611-55db-42e0-b01a-39559a3f1f2e-18846868.jpg)

          - Bean对象定义

      - springboot

        - XML文件

          - 1、创建类

          - 2、创建配置文件（src/main/resources目录下）

          - 3、在配置类或启动类中添加注解（只会有一个容器）

            - 启动类
              @SpringBootApplication
              @ImportResource("classpath:beans.xml")
              ​public class YourSpringBootApplication {
              ​    public static void main(String[] args) {
              ​        SpringApplication.run(YourSpringBootApplication.class, args);
              ​    }
              ​}

            - 配置类
              @Configuration
              @ImportResource("classpath:beans.xml")
              ​public class XmlConfiguration {
              ​    // 这里可以不写任何内容
              ​}

        - 注解

          - 1、直接添加注解

          - 2、注解类目录必须在规定目录下

        - 配置类：@ Configuration + @ Bean

  - 面向切面编程AOP

    - 理论/原理：（将影响多个类的公共行为封装到一个可重用模块）

      - 作用：利用一种称为"横切"的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，并将其命名为"Aspect"，即切面。所谓"切面"，简单说就是那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性

      - 应用：认证、日志、事务、异常![img](https://api2.mubu.com/v3/document_image/3ba0ed27-d9a5-4b84-bf9a-2f13cce2796d-18846868.jpg)

      - 代理模式

        - 静态代理

        - 动态代理

    - A O P 应用：

      - 步骤：

        - 1、添加依赖

        - 2、创建实现类

        - 3、创建切面类

        - 4、定义相关方法

          - 切入点表达式

          - 传参

          - 方法返回值

          - 异常

        - 5、配置类开启aop

      - 切入点方法：

      - 切入点表达式：

      - 传参：

  - 事务

- SpringMVC

  - 原理：
    - 流程图![img](https://api2.mubu.com/v3/document_image/31f007a2-0406-45b4-85b1-9b1bcb4d1c15-18846868.jpg)

  - 应用

    - SSM项目：

      - 步骤：

        - 1、依赖

        - 2、控制类

        - 3、配置类

        - 4、环境搭建

        - 5、tomcat运行

    - springboot项目：springMVC