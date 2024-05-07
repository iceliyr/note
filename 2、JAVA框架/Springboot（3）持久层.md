- 配置数据库连接

  - Mysql8.0：application.properties文件（必选属性）
    spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
    spring.datasource.url=jdbc:mysql://[localhost:3306/school](http://localhost:3306/school)
    spring.datasource.username=root
    ​spring.datasource.password=123456

  - MySql7.0：application.properties文件（必选属性）
    spring.datasource.driverClassName=com.mysql.jdbc.Driver
    ​spring.datasource.url=jdbc:mysql:///books
    ​spring.datasource.username=root
    ​spring.datasource.password=123456

- 配置数据库连接池

  - 属性

    - spring.datasource.url: 数据库连接URL。你需要指定MySQL服务器的主机名、端口号、数据库名称以及其他连接参数，如时区、SSL等。

    - spring.datasource.username和spring.datasource.password: 数据库用户名和密码，用于认证连接MySQL数据库。

    - spring.datasource.driver-class-name: 数据库驱动类名。对于MySQL 8.0，通常使用com.mysql.cj.jdbc.Driver。

    - spring.datasource.hikari.maximum-pool-size（可选）: 最大连接池大小。这个属性设置连接池中允许的最大连接数，默认为10。

    - spring.datasource.hikari.minimum-idle（可选）: 连接池保持空闲的最小连接数。当连接池中的连接数低于这个值时，将创建新的连接以满足需求。默认为10。

    - spring.jpa.database-platform（可选）: JPA方言类型。在这里，我们使用org.hibernate.dialect.MySQL8Dialect作为MySQL 8.0版本的方言。

    - spring.jpa.show-sql（可选）: 是否在日志中打印生成的SQL语句。设置为true将在控制台输出JPA生成的SQL语句，便于调试和分析。

    - logging.level.org.springframework.web（可选）: 日志级别。在这个示例中，我们将org.springframework.web包的日志级别设置为DEBUG，以便在控制台上获取更详细的日志信息。

  - 作用

    - 数据库连接池是个容器，负责分配、管理数据库连接(Connection)

    - 它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个

    - 释放空闲时间超过最大空闲时间的连接，来避免因为没有释放连接而引起的数据库连接遗漏

  - druid连接池：[Spring Boot3.0：数据库连接池Druid](https://zhuanlan.zhihu.com/p/636184214)

  - Hikari（springboot默认）![img](https://api2.mubu.com/v3/document_image/0469e88e-a654-4f6b-9e0b-17a8ad201395-18846868.jpg)

  - Druid

    - 依赖![img](https://api2.mubu.com/v3/document_image/76d434ae-4b72-443c-863b-28c093f5f51a-18846868.jpg)

    - 配置![img](https://api2.mubu.com/v3/document_image/351c8f0c-e878-4ff3-8681-732825265b59-18846868.jpg)

  - 属性

    

    - \#驱动 spring.datasource.driverClassName = com.mysql.jdbc.Driver

    - \#数据库链接spring.datasource.url = jdbc:mysql://[localhost:3306/testdb](http://localhost:3306/testdb)

    - \#用户名spring.datasource.username = root

    - \#密码spring.datasource.password = 123456

    - \#数据库连接池配置

    - \#初始化链接数spring.datasource.initialSize=5  

    - \#最小的空闲连接数spring.datasource.minIdle=5

    - \#最大的空闲连接数spring.datasource.maxIdle=20

    - \#最大活动连接数spring.datasource.maxActive=20

    - \#从池中取连接的最大等待时间，单位ms.  spring.datasource.maxWait=60000

    - \#每XXms运行一次空闲连接回收器spring.datasource.timeBetweenEvictionRunsMillis=60000 

    -  \#池中的连接空闲XX毫秒后被回收 spring.datasource.minEvictableIdleTimeMillis=300000  

    - \#验证使用的SQL语句  spring.datasource.validationQuery=SELECT 1 FROM DUAL  

    - \#指明连接是否被空闲连接回收器(如果有)进行检验.如果检测失败,则连接将被从池中去除.    spring.datasource.testWhileIdle=true  

    - \#借出连接时不要测试，否则很影响性能  spring.datasource.testOnBorrow=false  

    - \#归还连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能spring.datasource.testOnReturn=false  

    - \#是否缓存preparedStatement，也就是PSCache。PSCache对支持游标的数据库性能提升巨大，比如说oracle。在mysql5.5以下的版本中没有PSCache功能，建议关闭掉。5.5及以上版本有PSCache，建议开启。spring.datasource.poolPreparedStatements=true  

    - \#属性类型是字符串，通过别名的方式配置扩展插件，常用的插件有：监控统计用的filter:stat 日志用的filter:log4j 防御sql注入的filter:wallspring.datasource.filters=stat,wall,log4j 

    -  \#数据池连接参数spring.datasource.connectionProperties=druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000  

- MybatisX（插件）

  - 文档：[https://baomidou.com/pages/ba5b24/#%E5%8A%9F%E8%83%BD](https://baomidou.com/pages/ba5b24/#功能)

  - 安装并配置数据库

    - 1、安装插件![img](https://api2.mubu.com/v3/document_image/eb56e51a-0a8c-4687-91db-8dd439604d0e-18846868.jpg)

    - 2、添加数据库![img](https://api2.mubu.com/v3/document_image/7a001b66-6eaf-46b0-964f-f36318a9114f-18846868.jpg)

    - 3、连接数据库![img](https://api2.mubu.com/v3/document_image/3816481d-0bb7-4580-bcbb-1e476521d003-18846868.jpg)

    - 4、选择数据库![img](https://api2.mubu.com/v3/document_image/750b4871-5248-4cba-bf7f-5469b5717292-18846868.jpg)

  - 逆向工程（表创建实体类）

    - 1、选择表![img](https://api2.mubu.com/v3/document_image/304e70bf-609d-41e5-9e5b-82412782e12e-18846868.jpg)

    - 2、选择路径![img](https://api2.mubu.com/v3/document_image/7e072fa9-7bb1-459b-93fa-af1118d63e79-18846868.jpg)

    - 3、选择配置内容![img](https://api2.mubu.com/v3/document_image/f32dd9cd-448a-401f-839e-b52bebf64b42-18846868.jpg)

- Mybatis（基于sql语句）

  - 文档：[mybatis – MyBatis 3 ](https://mybatis.org/mybatis-3/zh/index.html)

  - 配置文件：

    - 驼峰命名：mybatis.configuration.mapUnderscoreToCamelCase=true

    - 配置 MyBatis 的 Mapper XML 文件路径：mybatis.mapperLocations=classpath:mapper/*.xml

  - 传入参数

    - 函数参数

      - 单变量
        - 直接引用

      - 多变量
        - @Param注解![img](https://api2.mubu.com/v3/document_image/bd6fde9f-4f2d-481e-8711-140a5a01dbb2-18846868.jpg)

      - 类变量：驼峰命名

        - 实体类属性：首单词小写，后面单词大写

        - 数据表属性：单词都小写，中间用"_"隔开

      - key-value变量
        - key当变量，value当值![img](https://api2.mubu.com/v3/document_image/18128b76-af0e-40d8-8b95-5ecef676e581-18846868.jpg)

      - 集合变量

        - 参数：集合、数组

        - 用于foreach

      - 集合类变量
        - foreach使用

    - sql语句变量

      - \#{}：转换为问号占位符

      - ${}：字符串拼接操作（少用）

    - 参数映射到变量

      - 数据封装

        - 实体类属性名和数据库表查询返回的字段名一致，mybatis会自动封装。

        - 如果实体类属性名 和 数据库表查询返回的字段名不一致，不能自动封装。![img](https://api2.mubu.com/v3/document_image/3f839b35-c51a-4c88-a17a-a584c5975b25-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/841b9fa6-9e8b-451d-8258-688516ba504e-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/9e8a91e8-1e54-44c7-98c5-5c632c842a10-18846868.jpg)

      - 解决封装问题

        - 起别名：在SQL语句中，对不一样的列名起别名，别名和实体类属性名一样。![img](https://api2.mubu.com/v3/document_image/cf156435-b85f-4d8a-ad36-fe1fd952d8a2-18846868.jpg)

        - 手动结果映射：通过 @Results及@Result 进行手动结果映射。![img](https://api2.mubu.com/v3/document_image/c0525b5f-fdd7-4ae1-bc22-33aaa22a01d1-18846868.jpg)

        - 开启驼峰命名：如果字段名与属性名符合驼峰命名规则，mybatis会自动通过驼峰命名规则映射。![img](https://api2.mubu.com/v3/document_image/aee2433e-223c-48f9-97c0-b982f1bf32bf-18846868.jpg)

  - 输出结果

    - 单变量：

      - return：int

      - resultType="int"

    - 多变量（类）：

      - return：类

      - resultType="全路径.类名"

    - Map：

      - key：属性，value：属性值

        - return：Map<String, Object>

        - resultType="map"

      - key：属性值，value：实体

        - 方法：![img](https://api2.mubu.com/v3/document_image/e8781882-c91d-4d50-b657-31aaecdc0e31-18846868.jpg)

        - xml文件：resultType="map"

    - List：

      - 变量

        - ArrayList<String> 

        - resultType="String"

      - 实体

        - List<User>

        - resultType="全路径类名"

    - 对一实体

      - 类定义![img](https://api2.mubu.com/v3/document_image/954f2920-e2ce-4798-8689-b42b98c35f98-18846868.jpg)

      - xml创建（association、javaType）

        

        - 创建resultMap：对应返回的数据类型

        - 处理属性

          - 普通属性直接映射

          - 实体属性：association，对应全路径类名
            - 对应具体类型

    - 对多实体

      - 实体![img](https://api2.mubu.com/v3/document_image/6c7f9c45-ec9f-4104-b20a-0428e9bca002-18846868.jpg)

      - xml（collection、ofType）![img](https://api2.mubu.com/v3/document_image/1fea09fd-7ce1-4345-afee-0ec9c19f3bea-18846868.jpg)

  - 注解方式

    - 基础步骤

      - 0、引入依赖和配置数据库信息

        - 引入Mybatis的相关依赖![img](https://api2.mubu.com/v3/document_image/332762a4-b7d0-4940-a377-99e21a71527a-18846868.jpg)
          \#驱动类名称
          spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
          \#数据库连接的url
          spring.datasource.url=jdbc:mysql://[localhost:3306/mybatis](http://localhost:3306/mybatis)
          \#连接数据库的用户名
          spring.datasource.username=root
          \#连接数据库的密码
          spring.datasource.password=123456

        - 配置Mybatis(数据库连接信息)![img](https://api2.mubu.com/v3/document_image/4a5d8dbe-4dfb-448a-a491-f742c3323723-18846868.jpg)

      - 1、定义一个类接收数据

      - 2、定义一个Mapper类处理数据

        

        - @ Mapper注解表示使用mybatis

        - @ Select("查询语句")  注解的变量就是返回的数据

      - 3、输出数据![img](https://api2.mubu.com/v3/document_image/6132fd1d-1752-40c2-8b93-d99e6aebc3fb-18846868.jpg)

    - 更新操作

      - 增![img](https://api2.mubu.com/v3/document_image/b506cf1a-21b3-45b0-b185-590f610a7ec4-18846868.jpg)

      - 删 ![img](https://api2.mubu.com/v3/document_image/aa589cfe-4783-4def-9d15-08a2ef99d4a5-18846868.jpg)

      - 改![img](https://api2.mubu.com/v3/document_image/77854cf6-b5d1-4e43-a6f9-7696892f6b85-18846868.jpg)

    - 查询操作
      - 一般查询![img](https://api2.mubu.com/v3/document_image/2e358448-05cc-46ba-b70b-b81607d89150-18846868.jpg)

    - 应用

      - 主键返回：（@ Options） 将主键值返回给emp.id![img](https://api2.mubu.com/v3/document_image/a2f33993-2ff0-4495-83e8-ef5a5c2503a3-18846868.jpg)

      - 模糊查询

        - 不能直接使用#，因为预编译会将其变为？号

        - 语法： 属性 like concat('前面符号'  ,   # {变量}  ,  '后面符合')![img](https://api2.mubu.com/v3/document_image/30d402da-025e-40f3-a9a5-96dd34cc1cf5-18846868.jpg)

  - XML 方式

    - 基础步骤

      - 1、XML映射文件的名称与Mapper接口名称一致，并且将XML映射文件和Mapper接口放置在相同包下（同包同名）![img](https://api2.mubu.com/v3/document_image/f9b4768b-a495-45cf-8b81-614c7feddd96-18846868.jpg)

      - 2、XML映射文件的namespace属性为Mapper接口全限定名一致

        

        <?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE mapper
                PUBLIC "-//[mybatis.org//DTD](http://mybatis.org//DTD) Mapper 3.0//EN"
                "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        ​<mapper namespace="com.itheima.mapper.EmpMapper">
        ​</mapper>

        - namespace="对应mapper类路径"

      - 3、XML映射文件中sql语句的id与Mapper 接口中的方法名一致，并保持返回类型一致。

        - xml文件![img](https://api2.mubu.com/v3/document_image/900836b9-b50d-4d29-9f1c-2dd52549cff3-18846868.jpg)

        - Mapper类中![img](https://api2.mubu.com/v3/document_image/d884eabe-eab7-4774-aee6-1a3fc1b0b1b8-18846868.jpg)

      - 4、运行

    - 增删改查

      - 增

        - 自增长

          - 语句

            

            - useGeneratedKeys属性字面意思就是“使用生成的主键”

            - keyProperty属性可以指定主键在实体类对象中对应的属性名，Mybatis会将拿到的主键值存入这个属性

          - 获取id：会自动填充到实体，所以直接调用![img](https://api2.mubu.com/v3/document_image/05b01220-7d0d-4016-b7f8-e15250cb0d8e-18846868.jpg)

        - 非自增长
          - 语句（UUID字符为36）![img](https://api2.mubu.com/v3/document_image/4c37c766-4a3e-4bae-b6bc-7ff4081079b5-18846868.jpg)

      - 改

        - 基本语法![img](https://api2.mubu.com/v3/document_image/81dee879-7fe3-4129-a59e-256ce1787f09-18846868.jpg)

        - <set></set>表示修改属性

        - <if></if>：根据判断结果选择sql语句

      - 删
        - 基本语法![img](https://api2.mubu.com/v3/document_image/2e25b797-a2f6-452c-9c58-548223993af6-18846868.jpg)

      - 查询

        - 属性：

          - id表示方法名

          - resultType表示返回对象数据类型

        - where/if

          

          

          - <where></where>表示查询内容，自动删掉多余的and和or

          - <if></if>根据判断选择要不要sql语句

    - XML属性

      - 常用![img](https://api2.mubu.com/v3/document_image/3ee7b5de-141a-4aeb-9dd8-b2a3366aef97-18846868.jpg)

      - 表与实体对应（resultMap）
        - resultMap![img](https://api2.mubu.com/v3/document_image/d8e96082-1859-4bbc-9b35-12d0813c0e81-18846868.jpg)

    - XML动态语法

      - trim：动态增加删除前后语句

        - 属性![img](https://api2.mubu.com/v3/document_image/cd463fad-a92b-41e5-8e81-61e2af65d87f-18846868.jpg)

        - 语法![img](https://api2.mubu.com/v3/document_image/b75dee52-66e0-4e1d-827a-8bd20ed77ca9-18846868.jpg)

      - choose、when、otherwise：只选择一个条件
        - 语法![img](https://api2.mubu.com/v3/document_image/122ce703-7522-471e-a28e-fea1b2d3046f-18846868.jpg)

      - <foreach></foreach>：遍历集合

        - 基本语法：delete from emp where id in(?,?,...,?,?);![img](https://api2.mubu.com/v3/document_image/5e08049d-6323-44da-bcdb-59f6567129cb-18846868.jpg)

        - collection：集合名称

        - item：集合遍历出来的元素/项

        - index：下标

        - separator：每一次遍历使用的分隔符

        - open：遍历开始前拼接的片段

        - close：遍历结束后拼接的片段

      - <include></include>：调用封装重复使用的sql语句

  - 插件

- Mybatis-Plus（基于对象）

  - 官方网站：[MyBatis-Plus](https://baomidou.com/pages/24112f/#特性)

  - 黑马笔记：https://b11et3un53m.feishu.cn/wiki/PsyawI04ei2FQykqfcPcmd7Dnsc

  - 基础：

    - 基本步骤

      - 1、配置数据库信息

      - 2、添加依赖（需要删除mybatis的依赖）
        <!--mybatis-plus的springboot支持 -->
        ​<dependency> 
        ​   <groupId>com.baomidou</groupId> 
        ​   <artifactId>mybatis-plus-boot-starter</artifactId>
        ​    <version>3.5.3.1</version>
        ​</dependency>
        ​<!-- mybatis-plus代码生成器 -->
        ​<dependency>
        ​    <groupId>com.baomidou</groupId>
        ​    <artifactId>mybatis-plus-generator</artifactId>
        ​    <version>3.5.3.1</version>
        ​</dependency>
        ​<!-- mybatisPlus Freemarker 模版引擎 -->
        ​<dependency> 
        ​   <groupId>org.freemarker</groupId>
        ​    <artifactId>freemarker</artifactId>
        ​    <version>2.3.31</version>
        ​</dependency>

      - 3、mybatis-plus全局配置（application）

      - 4、创建类并配置相关信息![img](https://api2.mubu.com/v3/document_image/a03cb72d-6368-4f1c-af98-f70358752885-18846868.jpg)

      - 5、创建相关mapper接口![img](https://api2.mubu.com/v3/document_image/866553de-25bc-47b3-ae34-bf7e9067d7e7-18846868.jpg)

      - 6、创建相关servive继承类![img](https://api2.mubu.com/v3/document_image/50a6acdc-29d4-45c8-b11e-ee1a793ae6b1-18846868.jpg)

    - 添加依赖：

      - 基础依赖
        <!--mybatis-plus的springboot支持 -->
        ​<dependency>
        ​    <groupId>com.baomidou</groupId>
        ​    <artifactId>mybatis-plus-boot-starter</artifactId>
        ​    <version>3.5.3.1</version>
        ​</dependency>

      - 自动生成代码：

        - 代码生成器
          <!-- mybatis-plus代码生成器 -->
          ​<dependency>
          ​    <groupId>com.baomidou</groupId>
          ​    <artifactId>mybatis-plus-generator</artifactId>
          ​    <version>3.5.3.1</version></dependency>

        - 模板引擎
          ​<!-- mybatisPlus Freemarker 模版引擎 -->
          ​<dependency>
          ​    <groupId>org.freemarker</groupId>
          ​    <artifactId>freemarker</artifactId>
          ​    <version>2.3.31</version>
          ​</dependency>

    - 配置文件：

      - 配置![img](https://api2.mubu.com/v3/document_image/d370e47a-68bd-42f6-9140-fa81f539f9f0-18846868.jpg)

      - mybatis-plus.configuration.logImpl=org.apache.ibatis.logging.stdout.StdOutImpl
        - 指定MyBatis-Plus框架在运行时使用的日志输出实现，这里选择了StdOutImpl实现，即将日志信息输出到控制台

      - mybatis-plus.configuration.mapUnderscoreToCamelCase=true
        - 指定MyBatis-Plus框架在与数据库进行交互时，自动将下划线命名的表字段转换为驼峰式命名的Java对象属性。例如，如果数据库中的表字段名为create_time，则会自动转换为Java对象属性名createTime

    - 实体类映射

      - 自动扫描

        - 类名驼峰转下划线作为表名

        - 名为id的字段作为主键

        - 变量名驼峰转下划线作为表的字段名

      - @ TableName：用来指定表名

      - @ TableId：用来指定表中的主键字段信息

        - 语法：@ TableId(value="主键列名",type=主键策略)

        - type：

          

          - AUTO：数据库自增长

          - INPUT：通过set方法自行输入

          - ASSIGN_ID：分配 ID，接口IdentifierGenerator的方法nextId来生成id，默认实现类为DefaultIdentifierGenerator雪花算法

            - 雪花算法：需要使用Long 或者 String类型主键

              - 组成：![img](https://api2.mubu.com/v3/document_image/2b5fe73e-e04e-4b6a-b0c3-363388e0d6fc-18846868.jpg)

              - 工作方式：![img](https://api2.mubu.com/v3/document_image/85fb4543-43b2-468b-a1a2-ec4e6ba1d817-18846868.jpg)

      - @ TableField：用来指定表中的普通字段信息

        - 使用场景：

        - 成员变量名与数据库字段名不一致

        - 成员变量名以is开头，且是布尔值

        - 成员变量名与数据库关键字冲突

        - 成员变量不是数据库字段

    - 方法

      - BaseMapper（封装Mapper）

      - ServiceImpl（封装Service）
        - 方法![img](https://api2.mubu.com/v3/document_image/7e3849d8-252d-4051-918f-44cc146e6191-18846868.jpg)![img](https://api2.mubu.com/v3/document_image/5cd21d98-430f-43af-9bca-36a08edd1d55-18846868.jpg)

      - IService（ServiceImpl实现）

    - 参数

      - Collection（集合处理）
        - 多值处理![img](https://api2.mubu.com/v3/document_image/fa02cd96-35f7-4d28-be62-604a985439cd-18846868.jpg)

      - Map（key-value条件处理）
        - 根据key值自动绑定条件![img](https://api2.mubu.com/v3/document_image/f4a5ab9a-437d-40b3-a604-af6202d1bed5-18846868.jpg)

      - Wrapper（条件构造器）[条件构造器](https://baomidou.com/pages/10c804/)

        - 步骤：

          - 1、创建条件构造器对象![img](https://api2.mubu.com/v3/document_image/fe6dfb67-81a3-4bf0-8ce5-77a1193300b6-18846868.jpg)

          - 2、添加条件![img](https://api2.mubu.com/v3/document_image/60c49b18-7bf8-40bd-8d93-dbeaafd8ce73-18846868.jpg)

          - 3、执行![img](https://api2.mubu.com/v3/document_image/06223890-5c3e-460b-a771-01785ab648d4-18846868.jpg)

        - 继承图

          

          - Wrapper ： 条件构造抽象类，最顶端父类

            - AbstractWrapper ： 用于查询条件封装，生成 sql 的 where 条件

              - QueryWrapper ： 查询/删除条件

              - UpdateWrapper ： 修改条件封装

              - UpdateWrapper ： 修改条件封装

                - LambdaQueryWrapper ：用于Lambda语法使用的查询Wrapper

                - LambdaUpdateWrapper ： Lambda 更新封装Wrapper

        - 条件语句![img](https://api2.mubu.com/v3/document_image/633a3055-5309-4606-a01c-0ef31eea0036-18846868.jpg)

        - QueryWrapper 
          - 追加条件![img](https://api2.mubu.com/v3/document_image/05f8f4be-6ade-4624-8317-aaf55dda7825-18846868.jpg)

        - UpdateWrapper

          - 使用queryWrapper:![img](https://api2.mubu.com/v3/document_image/85288b0d-29d3-4a54-9b02-7e05d8939aa2-18846868.jpg)

          - 使用updateWrapper:![img](https://api2.mubu.com/v3/document_image/ea9e994b-2508-4511-bef8-fc209af75fd1-18846868.jpg)

        - LambdaQueryWrapper

          - QueryWrapper![img](https://api2.mubu.com/v3/document_image/90ae1cb6-a1cb-4907-a5e7-02475891a4b0-18846868.jpg)

          - LambdaQueryWrapper ![img](https://api2.mubu.com/v3/document_image/92e349d9-7ee5-4cbb-ac3f-5382ffe54d05-18846868.jpg)

        - LambdaUpdateWrapper
          - 语法![img](https://api2.mubu.com/v3/document_image/45bdc468-2e7f-4ac2-b20a-449c393935bd-18846868.jpg)

  - 高级

    - 分页查询

      - 基础分页知识

        - 基本步骤：

          - 0、前端![img](https://api2.mubu.com/v3/document_image/ed8ca62f-1a40-41a2-94b1-f26c9bde496d-18846868.jpg)

          - 1、创建bean对象![img](https://api2.mubu.com/v3/document_image/0318897b-9fb8-4dde-bdab-06dc0228c804.png)

          - 2、接口类![img](https://api2.mubu.com/v3/document_image/72843ece-42ed-402a-8eed-59de06e13941-18846868.jpg)

          - 3、服务类![img](https://api2.mubu.com/v3/document_image/372fd655-d2c5-4ce1-818b-0616c5784062-18846868.jpg)

        - Bean对象：
          @BeanMybatisPlusInterceptor 
          mybatisPlusInterceptor(){
              MybatisPlusInterceptor interceptor=new MybatisPlusInterceptor();
              interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
          ​    return interceptor;
          ​}

        - Page对象：

          - 属性

            - records：获取当前分页查询结果集合，即当前页包含的数据记录

            - total：获取总记录数，即满足查询条件的总记录数

            - pages：总页数

            - size：获取每页显示的记录数

            - current：获取当前页码数

          - 构造器：

          - 方法：![img](https://api2.mubu.com/v3/document_image/0c3f2f84-2134-4652-914d-fa798259eaff-18846868.jpg)

        - mapper使用page

          - 返回Page<T>：selectPage(page对象，queryWrapper对象)
            - List<T> list = page.getRecords();

          - 返回Page<Map<String, Object>>：返回selectMapsPage(page对象，queryWrapper对象)
            - List<Map<String, Object>> records = page.getRecords();

      - 通用分页实体：https://www.bilibili.com/video/BV1Xu411A7tL?p=19

        - 1、定义模板

          - 查询模板

          - 结果模板

        - 2、使用步骤

          - 1、创建实现具体实体类的service方法

          - 2、创建接口，调用service方法

    - 逻辑删除：假删除，将对应数据中代表是否被删除字段的状态修改为“被删除状态”，之后在数据库中仍旧能看到此条数据记录

      - 数据库添加字段：
        - ALTER TABLE USER ADD deleted INT DEFAULT 0 ;  # int 类型 1 逻辑删除 0 未逻辑删除

      - 添加逻辑删除字段：

        - 局部（逻辑删除：1，未逻辑删除：0）![img](https://api2.mubu.com/v3/document_image/d51af54a-91a8-4ef5-8f5c-481bb6c409e5-18846868.jpg)

        - 全局![img](https://api2.mubu.com/v3/document_image/c4859065-c38d-4668-816e-266eab915e7d-18846868.jpg)

      - 删除字段：

        - 1、删除注解

        - 2、删除表字段：ALTER TABLE your_table DROP COLUMN deleted;

    - 乐观锁：

      - 基本步骤：

        - 1、创建bean对象
          @Bean
          public MybatisPlusInterceptor mybatisPlusInterceptor() {
              MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
              interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
              return interceptor;
          }

        - 2、添加字段
          ALTER TABLE USER ADD VERSION INT DEFAULT 1 ;  # int 类型 乐观锁字段

        - 3、添加注解
          @Version
          private Integer version;

        - 4、更新（与正常一样）

    - 全表更新与删除：阻止恶意更新删除整个表
      - MybatisPlusInterceptor的bean对象添加：interceptor.addInnerInterceptor(new BlockAttackInnerInterceptor());
        @Bean
        public MybatisPlusInterceptor mybatisPlusInterceptor() {
          MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
          interceptor.addInnerInterceptor(new BlockAttackInnerInterceptor());
          return interceptor;
        }
        }

- Spring Data JPA（基于对象）

  - 意义：提高增删改查等常用功能，使开发者用较少的代码实现数据库操作，同时还易于扩展

  - 基础步骤（[Spring-data-jpa入门](https://blog.csdn.net/qq_42495847/article/details/107991361)）（[spring-data-jpa入门（二）](https://blog.csdn.net/qq_42495847/article/details/108801387)）

    - 1、基础准备：添加依赖、创建数据库、application配置文件配置数据库信息

    - 2、实体类映射数据库表

    - 3、创建repository接口

    - 4、service直接调用repository继承类

  - 实体类映射数据库表

    - 注解（[JPA中实体类属性相关注解与数据表列映射](https://blog.csdn.net/J080624/article/details/78745962)）![img](https://api2.mubu.com/v3/document_image/edeed11d-cab7-4377-95de-2f3396d68d89-18846868.jpg)

    - 举例（https://blog.51cto.com/u_16175487/6946080）![img](https://api2.mubu.com/v3/document_image/37106745-3c2e-4aaf-aba1-29307d0e8b5f-18846868.jpg)

  - 创建repository接口继承类

    - 语法

      

      - @Modifying：标记某个方法执行的是修改操作（INSERT、UPDATE 或 DELETE）

      - @Transactional：指定该方法在一个事务中运行，以保证数据操作的一致性和完整性

      - @Query：自定义查询语句，用于对数据库进行查询和操作。可以使用 JPQL 或 SQL 语句进行查询操作

    - 方法

      - 默认方法![img](https://api2.mubu.com/v3/document_image/4478ace1-c7ab-424a-a551-816aae020c2c-18846868.jpg)

      - 方法名解析![img](https://api2.mubu.com/v3/document_image/f558de1a-b732-415e-ad34-57e609607cd5-18846868.jpg)

      - @Query 注解方法（里面添加sql语句）

        - 方式1：

          

          - sql语句:#{#user.getXXX()}

          - 方法：实体对象

  - 接口（[SpringData JPA](https://zhuanlan.zhihu.com/p/624207419)  、 [Spring Data JPA 中文文档 ](https://springdoc.cn/spring-data-jpa/)）

    

    - Repository接口是Spring Data JPA提供的用于自定义Repository接口的顶级父接口，该接口没有声明任何方法。

    - CrudRepository接口是Repository的继承接口之一，包含一些基本的CRUD方法。![img](https://api2.mubu.com/v3/document_image/33fa3a78-614e-43c8-a15a-aded14b5166c-18846868.jpg)

    - PagingAndSortingRepository接口继承了CrudRepository接口的同时，提供分页和排序两个方法![img](https://api2.mubu.com/v3/document_image/2faf141d-e2a0-4b4a-b045-34c4911272c7-18846868.jpg)

    - QueryByExampleExecutor接口是进行条件封装查询的顶级父接口，运行通过Example实例执行复杂的条件查询。

    - JpaRepository接口同时继承了PagingAndSortingRepository接口和QueryByExampleExecutor接口，并额外提供了一些数据操作方法，自定义Repository接口时，通常可以直接选择继承JpaRepository接口。

- Hibernate（基于对象）

  - 作用：JPA的具体实现

  - 文档：[Hibernate 中文文档](https://hibernate.net.cn/)