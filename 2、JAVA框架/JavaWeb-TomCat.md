- 概述：web服务器

  - Web服务器是一个软件程序，对HTTP协议的操作进行封装，使得程序员不必直接对协议进行操作，让Web开发更加便捷。主要功能是 "提供网上信息浏览服务" 

  - Tomcat就是一个轻量级的web服务器，支持servlet、jsp等少量javaEE规范，也被称为web容器、servlet容器

- Windows部署

  - 下载：官网下载，地址 https://tomcat.apache.org/download-90.cgi

  - 安装：绿色版，直接解压即可（目录没中文）

  - 配置项目：放置于webapps目录![img](https://api2.mubu.com/v3/document_image/14bbe290-a9c2-4b14-a9cb-5e8cbc6f88e4-18846868.jpg)

  - 启动：双击：bin\startup.bat

  - 卸载：直接删除目录即可

  - 关闭：

    - 直接×掉运行窗口：强制关闭

    - bin\shutdown.bat：正常关闭

    - Ctrl+C：正常关闭

- Linux部署

  - https://blog.csdn.net/qq_40223688/article/details/88426631

- 目录文件

  - bin：二进制可执行文件

    - startup.bat用来启动Tomcat，但需要先配置JAVA_HOME环境变量才能启动

    - shutdawn.bat用来停止Tomcat

  - config

    - server.xml：配置整个服务器信息

    - tomcat-users.xml：存储tomcat用户的文件，这里保存的是tomcat的用户名及密码，以及用户的角色信息

    - web.xml：部署描述符文件，这个文件中注册了很多MIME类型，即文档类型。这些MIME类型是客户端与服务器之间说明文档类型的，如用户请求一个html网页，那么服务器还会告诉客户端浏览器响应的文档是text/html类型的，这就是一个MIME类型。客户端浏览器通过这个MIME类型就知道如何处理它了。当然是在浏览器中显示这个html文件了。但如果服务器响应的是一个exe文件，那么浏览器就不可能显示它，而是应该弹出下载窗口才对。MIME就是用来说明文档的内容是什么类型的！

    - context.xml：对所有应用的统一配置，通常我们不会去配置它

  - lib：Tomcat的类库，里面是一大堆jar文件
    - 如果需要添加Tomcat依赖的jar文件，可以把它放到这个目录中，当然也可以把应用依赖的jar文件放到这个目录中，这个目录中的jar所有项目都可以共享之，但这样你的应用放到其他Tomcat下时就不能再共享这个目录下的jar包了，所以建议只把Tomcat需要的jar包放到这个目录下；

  - logs：这个目录中都是日志文件，记录了Tomcat启动和关闭的信息，如果启动Tomcat时有错误，那么异常也会记录在日志文件中。

  - temp：存放Tomcat的临时文件，这个目录下的东西可以在停止Tomcat后删除

  - webapps：存放web项目的目录，其中每个文件夹都是一个项目
    - 如果这个目录下已经存在了目录，那么都是tomcat自带的项目。其中ROOT是一个特殊的项目，在地址栏中访问：[http://127.0.0.1:8080](http://127.0.0.1:8080/)，没有给出项目目录时，对应的就是ROOT项目.http://localhost:8080/examples，进入示例项目。其中examples"就是项目名，即文件夹的名字。

  - work：运行时生成的文件，最终运行的文件都在这里
    - 通过webapps中的项目生成的！可以把这个目录下的内容删除，再次运行时会生再次生成work目录。当客户端用户访问一个JSP文件时，Tomcat会通过JSP生成Java文件，然后再编译Java文件生成class文件，生成的java和class文件都会存放到这个目录下。

- 配置/操作

  - 控制台中文乱码：修改conf/ logging.properties![img](https://api2.mubu.com/v3/document_image/339204c8-3bdb-48b7-8c7c-6943638af847-18846868.jpg)

  - 配置Tomcat端口号（conf/server.xml）![img](https://api2.mubu.com/v3/document_image/f06cd33c-cd14-4c25-a1d4-d5990917cc5e-18846868.jpg)

- 项目目录结构

  - 标准结构

    

    - static：非必要目录，静态资源

    - WEB-INF  必要目录（受保护的资源目录,浏览器通过url不可以直接访问的目录）

      - classes     必要目录,src下源代码,配置文件,编译后会在该目录下,web项目中如果没有源码,则该目录不会出现

      - lib             必要目录,项目依赖的jar编译后会出现在该目录下,web项目要是没有依赖任何jar,则该目录不会出现

      - web.xml   必要文件,web项目的基本配置文件. 较新的版本中可以没有该文件,但是学习过程中还是需要该文件 

    - index.html  非必要文件,index.html/index.htm/index.jsp为默认的欢迎页

  - URL对应资源![img](https://api2.mubu.com/v3/document_image/92c53bfd-80ec-4c0e-9c83-57dd0756426e-18846868.jpg)

- 部署方式

  - 方式1   直接将编译好的项目放在webapps目录下

  - 方式2   将编译好的项目打成war包放在webapps目录下,tomcat启动后会自动解压war包

  - 方式3   可以将项目放在非webapps的其他目录下,在tomcat中通过配置文件指向app的实际磁盘路径
    - 在tomcat的conf下创建Catalina/[localhost](http://localhost/)目录,并在该目录下准备一个app.xml文件
      <!-- 
      	path: 项目的访问路径,也是项目的上下文路径,就是在浏览器中,输入的项目名称
          docBase: 项目在磁盘中的实际路径
       -->
      <Context path="/app" docBase="D:\mywebapps\app" />

  - 方式4   使用IDEA工具运行

- IDEA集成

  - IDEA关联Tomcat

    - 1、打开全局设置![img](https://api2.mubu.com/v3/document_image/99ff3d11-ac60-4ada-8d05-fb40fba43cac-18846868.jpg)

    - 2、找到 Build,Execution,Eeployment下的Application Servers ,找到+号![img](https://api2.mubu.com/v3/document_image/885a50d9-3597-4bfd-8378-096e10eae367-18846868.jpg)

    - 3、选择Tomcat Server![img](https://api2.mubu.com/v3/document_image/ca610a37-cb45-408f-833b-32a6f5518f47-18846868.jpg)

    - 4、选择tomcat的安装目录![img](https://api2.mubu.com/v3/document_image/a80f4307-8fe1-4654-b73b-2f872cd8176b-18846868.jpg)

    - 5、点击ok![img](https://api2.mubu.com/v3/document_image/ef82f9ec-ffff-4161-a8cf-659a3fb8d398-18846868.jpg)

    - 6、关联完毕![img](https://api2.mubu.com/v3/document_image/290c6f6c-444a-4ad9-8539-53524af0ecc6-18846868.jpg)

  - 创建web项目

    - 1、项目添加Tomcat依赖![img](https://api2.mubu.com/v3/document_image/c6fda29b-ad84-4d6a-a163-47b089d85fb7-18846868.jpg)

    - 2、添加  framework support![img](https://api2.mubu.com/v3/document_image/c5df2887-4285-411a-b7d6-158b9df59073-18846868.jpg)

    - 3、选择Web Application 注意Version,勾选  Create web.xml![img](https://api2.mubu.com/v3/document_image/cd0216e4-8bcb-4e68-8f29-101a4405ef48-18846868.jpg)

    - 4、查看项目结构![img](https://api2.mubu.com/v3/document_image/83ebc16c-9a32-4020-860b-8d49310a05d3-18846868.jpg)

    - 5、添加resources项目：专门用于存放配置文件(都放在src下也行,单独存放可以尽量避免文件集中存放造成的混乱)，标记目录为资源目录,不标记的话则该目录不参与编译![img](https://api2.mubu.com/v3/document_image/8420daa1-2c39-4296-ac8e-f43b41ecb62e-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/086c555a-965e-477b-9f5d-0f68a40ae5f2-18846868.jpg)

    - 6、处理jar包

      - 在WEB-INF下创建lib目录（必须在WEB-INF下,且目录名必须叫lib），复制jar文件进入lib目录![img](https://api2.mubu.com/v3/document_image/5b96c6ec-4550-408e-9b2d-b019fc7e9398-18846868.jpg)

      - 将lib目录添加为当前项目的依赖,后续可以用maven统一解决![img](https://api2.mubu.com/v3/document_image/c548dcf8-0cc4-4de1-a064-e8773141354b-18846868.jpg)

      - 环境级别推荐选择module 级别,降低对其他项目的影响,name可以空着不写![img](https://api2.mubu.com/v3/document_image/8b941523-631a-493d-9455-61289cea496f-18846868.jpg)

      - 查看当前项目有那些环境依赖![img](https://api2.mubu.com/v3/document_image/84315289-779d-4627-9faf-ac4ae5439f2b-18846868.jpg)

      - 可以通过-号解除依赖![img](https://api2.mubu.com/v3/document_image/64d26da6-73bf-4d46-b5a4-8c8a87a0c6eb-18846868.jpg)

  - 部署运行项目

    - 1、就是检查工程目录下,web目录有没有特殊的识别标记![img](https://api2.mubu.com/v3/document_image/9521be7d-addf-4042-b0b9-43ecd1f7c08b-18846868.jpg)

    - 2、以及artifacts下,有没有对应 _war_exploded,如果没有,就点击+号添加![img](https://api2.mubu.com/v3/document_image/870b6961-9367-4fab-94ae-646bf20751bc-18846868.jpg)

    - 3、点击向下箭头,出现 Edit Configurations选项![img](https://api2.mubu.com/v3/document_image/58e2f31b-2099-4934-b08a-12e1f5fb2226-18846868.jpg)

    - 4、点击+号,添加本地tomcat服务器![img](https://api2.mubu.com/v3/document_image/cf585afd-0b2a-483c-9b83-30f49eee60c4-18846868.jpg)

    - 5、因为IDEA 只关联了一个Tomcat,红色部分就只有一个Tomcat可选![img](https://api2.mubu.com/v3/document_image/328e5089-0b2c-4917-b9e2-b705ab9983b5-18846868.jpg)

    - 6、选择Deployment,通过+添加要部署到Tomcat中的artifact![img](https://api2.mubu.com/v3/document_image/7501dbb8-f8b0-47e4-a339-413f4bf8acaf-18846868.jpg)

    - 7、applicationContext中是默认的项目上下文路径,也就是url中需要输入的路径,这里可以自己定义,可以和工程名称不一样,也可以不写,但是要保留/,我们这里暂时就用默认的![img](https://api2.mubu.com/v3/document_image/41730e01-4de5-404b-b22f-ddfa847e9304-18846868.jpg)

    - 8、点击apply 应用后,回到Server部分. After Launch是配置启动成功后,是否默认自动打开浏览器并输入URL中的地址,HTTP port是Http连接器目前占用的端口号![img](https://api2.mubu.com/v3/document_image/4f9f587c-a078-4faa-b639-6a5d49f46aab-18846868.jpg)

    - 9、点击OK后,启动项目,访问测试

  - IDEA部署原理

    

    - idea并没有直接进将编译好的项目放入tomcat的webapps中

    - idea根据关联的tomcat,创建了一个tomcat副本,将项目部署到了这个副本中

    - idea的tomcat副本在C:\用户\当前用户\AppData\Local\JetBrains\IntelliJIdea2022.2\tomcat\中

    - idea的tomcat副本并不是一个完整的tomcat,副本里只是准备了和当前项目相关的配置文件而已

    - idea启动tomcat时,是让本地tomcat程序按照tomcat副本里的配置文件运行

    - idea的tomcat副本部署项目的模式是通过conf/Catalina/localhost/*.xml配置文件的形式实现项目部署的