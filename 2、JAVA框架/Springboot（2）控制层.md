- springMVC

  - 原理：

    

    

    - 组件![img](https://api2.mubu.com/v3/document_image/2e646a10-2dbb-4130-ae02-4c6469dd15be-18846868.jpg)

    - 访问类型

      - 静态文件：直接访问存储路径

      - 视图：根据路径及参数返回视图

      - 数据：根据路径返回对应数据

  - 应用

    - 类注解

      - Controller

        - 可以返回实体对象（JSON）

        - 可以返回HTML模板

      - ReseponseBody
        - 将controller的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML数据

      - RestController（Controller+ReseponseBody）
        - 只能返回String、Object、Json等实体对象

    - 路径

      - 类添加

      - 方法添加

    - 接收请求参数

      - 请求行

        - 参数名
          - 语法![img](https://api2.mubu.com/v3/document_image/045a1023-f825-4e75-8953-a32fe4f66e67-18846868.jpg)

        - @RequestParam

          - 语法：@RequestParam(value/name=”参数名”,required=”true/false”,defaultValue=””)

            - value/name：参数名

            - required：是否包含该参数，默认为true，表示该请求路径中必须包含该参数，如果不包含就报错。

            - defaultValue：默认参数值，如果设置了该值，required=true将失效，自动为false,如果没有传该参数，就使用默认值

          - 案例![img](https://api2.mubu.com/v3/document_image/6716abe2-3876-4418-931f-0f3d675553e1-18846868.jpg)

        - @PathVariable 路径数据
          - 语法![img](https://api2.mubu.com/v3/document_image/0f1786c9-56f1-4971-9cec-730fd92b5003-18846868.jpg)

        - 对象
          - 语法![img](https://api2.mubu.com/v3/document_image/f48fe084-754b-4401-8c37-52ca27e561b7-18846868.jpg)

        - 数组/集合
          - 语法![img](https://api2.mubu.com/v3/document_image/1eb80e9d-611d-4f84-913f-bd35cd16403a-18846868.jpg)

      - 请求头：

        - HttpServletRequest类
          - 语法![img](https://api2.mubu.com/v3/document_image/4a3d6a66-9711-4103-840d-3cd5fab5537b-18846868.jpg)

        - RequestHeader![img](https://api2.mubu.com/v3/document_image/c795f3ec-db7a-4c3e-9985-553b48fca599-18846868.jpg)

        - Cookie

          

          - 添加（key：Cookie  ，value：xxx=xxxx）

      - 请求体
        - RequestBody![img](https://api2.mubu.com/v3/document_image/ef76b7e8-6118-4c5d-ad85-ac7b99f5f991-18846868.jpg)

      - 格式
        - 日期
          - 语法![img](https://api2.mubu.com/v3/document_image/811b080d-dc0c-4c81-8d37-0c30cae509b9-18846868.jpg)

    - 共享域

      - request：数据在当前请求有效，请求转发后有效，重定向无效（[SpringMVC使用域](https://blog.csdn.net/weixin_44741023/article/details/119859102)）

        - HttpServletRequest

        - ModelAndView

        - Model

        - ModelMap

        - Map

      - session：数据在关闭浏览器前有效，中途关闭服务器，数据钝化（还在），重启浏览器数据又会活化（还能用）

        - HttpServletRequest![img](https://api2.mubu.com/v3/document_image/83d451e3-3c85-4f25-a5e7-028e3cccfc1c-18846868.jpg)

        - HttpSession

      - application：数据在关闭服务器前有效（关闭浏览器数据还在）

    - HttpServletRequest和HttpServletResponse
      - 方法：[HttpServletRequest类详解](https://blog.csdn.net/f2764052703/article/details/89432259)![img](https://api2.mubu.com/v3/document_image/639baa2c-6d8c-4b2e-9cd4-d4a1222b6c0e-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/ed7c6306-d91b-4671-a400-7ed1011bb2f9-18846868.jpg)

    - 响应输出参数

      - 输出json等参数

      - 输出模板

    - restful规范

      - 接口方法

        - 接口路径：实体类名/方法/路径![img](https://api2.mubu.com/v3/document_image/d4dfb846-4ffe-4698-b030-4c5e62dfb40a-18846868.jpg)

        - 增（POST）

          - 接口：POST/实体类

          - 参数：请求体中

          - 后端：@ RequestBody   User user

        - 删（DELETE）
          - 接口：DELETE/实体类/{id}

        - 改（PUT）

          - 接口：PUT/实体类/{id}

          - 参数：请求体

          - 后端：@ RequestBody   User user

        - 查（GET）

          - 全部查询：GET/实体类

          - 分页查询：GET/实体类?limit=xx&offset=xx

          - 一个查询：GET/实体类/{id}

          - 条件查询：GET/实体类/?xxx=xxx

          - 模糊查询：GET/实体类/search?xxx=xxx

          - 多条件模糊查询：GET/实体类/search?xxx=xxx&xxx=xxx

      - 返回参数：

        - GET

        - 安全且幂等

        - 获取表示

        - 变更时获取表示（缓存）

        - 200（OK） - 表示已在响应中发出

        - 204（无内容） - 资源有空表示

        - 301（Moved Permanently） - 资源的URI已被更新

        - 303（See Other） - 其他（如，负载均衡）

        - 304（not modified）- 资源未更改（缓存）

        - 400 （bad request）- 指代坏请求（如，参数错误）

        - 404 （not found）- 资源不存在

        - 406 （not acceptable）- 服务端不支持所需表示

        - 500 （internal server error）- 通用错误响应

        - 503 （Service Unavailable）- 服务端当前无法处理请求

        - POST

        - 不安全且不幂等

        - 使用服务端管理的（自动产生）的实例号创建资源

        - 创建子资源

        - 部分更新资源

        - 如果没有被修改，则不过更新资源（乐观锁）

        - 200（OK）- 如果现有资源已被更改

        - 201（created）- 如果新资源被创建

        - 202（accepted）- 已接受处理请求但尚未完成（异步处理）

        - 301（Moved Permanently）- 资源的URI被更新

        - 303（See Other）- 其他（如，负载均衡）

        - 400（bad request）- 指代坏请求

        - 404 （not found）- 资源不存在

        - 406 （not acceptable）- 服务端不支持所需表示

        - 409 （conflict）- 通用冲突

        - 412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突）

        - 415 （unsupported media type）- 接受到的表示不受支持

        - 500 （internal server error）- 通用错误响应

        - 503 （Service Unavailable）- 服务当前无法处理请求

        - PUT

        - 不安全但幂等

        - 用客户端管理的实例号创建一个资源

        - 通过替换的方式更新资源

        - 如果未被修改，则更新资源（乐观锁）

        - 200 （OK）- 如果已存在资源被更改

        - 201 （created）- 如果新资源被创建

        - 301（Moved Permanently）- 资源的URI已更改

        - 303 （See Other）- 其他（如，负载均衡）

        - 400 （bad request）- 指代坏请求

        - 404 （not found）- 资源不存在

        - 406 （not acceptable）- 服务端不支持所需表示

        - 409 （conflict）- 通用冲突

        - 412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突）

        - 415 （unsupported media type）- 接受到的表示不受支持

        - 500 （internal server error）- 通用错误响应

        - 503 （Service Unavailable）- 服务当前无法处理请求

        - DELETE

        - 不安全但幂等

        - 删除资源

        - 200 （OK）- 资源已被删除

        - 301 （Moved Permanently）- 资源的URI已更改

        - 303 （See Other）- 其他，如负载均衡

        - 400 （bad request）- 指代坏请求

        - 404 （not found）- 资源不存在

        - 409 （conflict）- 通用冲突

        - 500 （internal server error）- 通用错误响应

        - 503 （Service Unavailable）- 服务端当前无法处理请求

    - 参数检验

      - 注解（注解在实体中）![img](https://api2.mubu.com/v3/document_image/788a733a-b53c-4738-a5c3-c6065274a84c-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/f6ae7957-7068-4f64-a74d-c7b2c342a03c-18846868.jpg)

      - 应用：

        - 1、依赖
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-validation</artifactId>
          </dependency>

        - 2、实体类创建![img](https://api2.mubu.com/v3/document_image/6c924126-8ec2-4de5-8dc6-be2034759dba-18846868.jpg)

        - 3、控制类使用![img](https://api2.mubu.com/v3/document_image/9192dbc4-94e9-4f8a-94e3-893a5d6905c7-18846868.jpg)

- 过滤器Filter（servlet、拦截全部资源）

  - 执行流程

    - 1、根据拦截路径调用相应的方法

    - 2.1、执行init

    - 3、执行dofilter方法前面

    - 4、执行dofilter方法放行

    - 5、执行dofilter方法后面

    - 2.2、执行destroy

  - 基本步骤

    - 1、定义类

    - 2、配置Filter：Filter类上加 @ WebFilter 注解，配置拦截资源的路径

    - 3、实现 Filter 接口，并重写其所有方法![img](https://api2.mubu.com/v3/document_image/18542024-b2ef-4e28-a1b5-be49efe0e75e-18846868.jpg)

    - 4、启动类上加 @ ServletComponentScan 开启Servlet组件支持![img](https://api2.mubu.com/v3/document_image/6f063e47-f622-445c-803c-3d4ef85d1bd2-18846868.jpg)

  - Filter

    - init

    - doFilter方法（放行）
      - filterChain.doFilter(servletRequest,servletResponse)：执行相应请求

    - destroy

  - 拦截路径

    - /* ：所有路径

    - /具体路径：具体路径

    - /目录/*：该目录所有资源

  - 过滤器链

    - 定义：一个web应用中，可以配置多个过滤器，这多个过滤器就形成了一个过滤器链

    - 顺序：根据类名排序（A->B）

- 拦截器Interceptor（springmvc、拦截spring资源）

  - 作用：拦截请求，在指定的方法调用前后，根据业务需要执行预先设定的代码

  - Interceptor执行步骤

    

    - 1、定义拦截器，实现HandlerInterceptor接口，并重写其所有方法

      - preHandle：统一拦截操作一般放在这里

        

        

        - return false：拦截

        - return true：放行

    - 2、注册拦截器

      

      - addPathPatterns：添加拦截资源路径

      - excludePathPatterns：去掉该拦截资源路径

  - 定义拦截器

    

    - preHandler：

      - return true：放行

      - return false：不放行

    - postHandle：

    - afterCompletion：

  - 注册拦截器

    - 自定义拦截器：addInterceptor
      - InterceptorRegistration interceptorRegistration = registry.addInterceptor(myInterceptor);![img](https://api2.mubu.com/v3/document_image/ecb4cdab-e6e6-4a75-b275-f58960dff6b5-18846868.jpg)

    - 拦截顺序：order（拦截器的执行顺序是根据指定的order值来确定的，值越小，拦截器的执行顺序越靠前）![img](https://api2.mubu.com/v3/document_image/b68587a4-ca23-4f1e-a397-e4a3498def9f-18846868.jpg)

    - 拦截路径：addPathPatterns（指定）/excludePathPatterns（排除）![img](https://api2.mubu.com/v3/document_image/ee98c543-590b-4194-b504-6fdcffa3a974-18846868.jpg)

- [全局异常处理](https://blog.csdn.net/qq_41107231/article/details/115874974)