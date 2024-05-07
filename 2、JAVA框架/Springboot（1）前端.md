- 前端目录

  - resources

    - static（静态文件）

    - templates（模板动态文件）
      - 引入静态文件![img](https://api2.mubu.com/v3/document_image/84b46c49-7f02-4aa2-956e-22b468484428-18846868.jpg)

- [Thymeleaf](https://fanlychie.github.io/post/thymeleaf.html)

  - 步骤

    - 1、添加依赖
      <!-- Thymeleaf 依赖 -->
      ​<dependency>
      ​    <groupId>org.springframework.boot</groupId>
      ​    <artifactId>spring-boot-starter-thymeleaf</artifactId>
      ​</dependency>

    - 2、创建html/css/js文件

      

      - 静态文件放在static

      - 模板文件放在templates

    - 3、controller控制类配置接口![img](https://api2.mubu.com/v3/document_image/2bfcf50f-e9c1-496e-bd35-afd1fe27f42f-18846868.jpg)
      @GetMapping("/index")
      public String index(){
      return "index";}

  - 前端

    - 标签

      - th:text![img](https://api2.mubu.com/v3/document_image/debb46f2-b958-47ed-acc5-d5454c4e50ac-18846868.jpg)

      - th:utext![img](https://api2.mubu.com/v3/document_image/629c9639-c81a-497d-82ee-9ef6a74ced31-18846868.jpg)

      - th:if![img](https://api2.mubu.com/v3/document_image/14530b97-8cec-493e-9895-d3f505333a9f-18846868.jpg)

      - th:unless![img](https://api2.mubu.com/v3/document_image/b3e39b69-e919-4ae6-922b-29995ab84337-18846868.jpg)

      - th:switch  和th:case![img](https://api2.mubu.com/v3/document_image/921d8c45-e18d-4e30-ba22-4d635f574cca-18846868.jpg)

      - th:action![img](https://api2.mubu.com/v3/document_image/cd639d1b-c5da-41fc-87ed-4c6f12791a9b-18846868.jpg)

      - th:each![img](https://api2.mubu.com/v3/document_image/f2d5c25e-d8cd-4208-a119-f06e063dfec5-18846868.jpg)

      - th:value![img](https://api2.mubu.com/v3/document_image/63c5256a-59e1-43c3-b00a-34ee12521ca4-18846868.jpg)

      - th:src![img](https://api2.mubu.com/v3/document_image/e5f823c1-dd92-4ba3-b0f4-818826fc1be7-18846868.jpg)

      - th:href![img](https://api2.mubu.com/v3/document_image/1a1ea949-db73-43de-b3ae-2b186858ad98-18846868.jpg)

      - th:selected![img](https://api2.mubu.com/v3/document_image/d25c5cad-8f77-486c-a2cc-2117c6a1e8f5-18846868.jpg)

      - th:object![img](https://api2.mubu.com/v3/document_image/f7f21904-43cb-40ba-99c1-2857a37c77f3-18846868.jpg)

      - th:attr![img](https://api2.mubu.com/v3/document_image/b3198ffa-66df-4eaa-8213-247fe9b7cd5f-18846868.jpg)

    - 表达式![img](https://api2.mubu.com/v3/document_image/4bdc11ea-beb8-4598-97e0-d49f4ef7d1ff-18846868.jpg)

  - 后端

    - 接收前端数据

    - 数据添加发送

    - 返回前端界面

    - 重定向与转发

      - redirect：适用于需要跳转到其他网站或Web应用程序，以及需要刷新页面的情况

      - forward：适用于在当前Web应用程序内部进行页面跳转，且需要保留上下文信息的情况

      - 区别

        - 执行时机：redirect是在服务器端发送响应给浏览器后，由浏览器发起新的请求进行跳转；而forward是在服务器端内部直接进行跳转，浏览器并不知道发生了跳转。

        - 请求方式：redirect是使用HTTP的重定向机制，即浏览器会发起一个新的GET请求；而forward是在服务器内部进行跳转，使用的是同一个请求。

        - URL变化：redirect会改变浏览器的URL地址，而forward不会改变URL地址，仍然显示原始请求的URL。

        - 上下文丢失：redirect会导致之前请求的上下文信息（包括请求参数、Session等）丢失，因为它是一个新的请求；而forward可以保留上下文信息，因为它是在同一个请求中进行跳转。

        - 跳转对象：redirect可以跳转到任意URL，包括外部网址；而forward只能跳转到当前Web应用程序内的其他资源（如servlet、JSP等），不能跳转到外部网址。

- JQuery

- ajax

- 接口方式

  - 方法+ajax

  - 就绪事件+ajax

  - 直接路径