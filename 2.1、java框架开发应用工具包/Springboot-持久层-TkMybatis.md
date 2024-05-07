### 一、什么是 TKMybatis

TKMybatis 是基于 Mybatis 框架开发的一个工具，内部实现了对单表的基本数据操作，只需要简单继承 TKMybatis 提供的接口，就能够实现无需编写任何 sql 即能完成单表操作。



### 二、TKMybatis 基础使用步骤

1、Springboot 项目中加入依赖
`<!--通用mapper起步依赖-->`
`<dependency>`
    `<groupId>tk.mybatis</groupId>`
    `<artifactId>mapper-spring-boot-starter</artifactId>`
    `<version>2.0.4</version>`
`</dependency>`

2、在 POJO 类中加入依赖

`<!--每个工程都有Pojo，都需要用到该包对应的注解-->`
`<dependency>`
    `<groupId>javax.persistence</groupId>`
    `<artifactId>persistence-api</artifactId>`
    `<version>1.0</version>`
    `<scope>compile</scope>`
`</dependency>`

3、在启动类中配置 @MapperScan 扫描

`@SpringBootApplication`
`@MapperScan(basePackages = {"com.tom.order.mapper"})`
`public class OrderApplication {`
    `public static void main(String[] args) {`
        `SpringApplication.run(OrderApplication.class, args);`
    `}`
`}`

4、 实体类中使用
在实体类中，常用的注解和意义为：

@Table：描述数据库表信息，主要属性有name(表名)、schema、catalog、uniqueConstraints等。

@Id：指定表主键字段，无属性值。

@Column：描述数据库字段信息，主要属性有name(字段名)、columnDefinition、insertable、length、nullable(是否可为空)、precision、scale、table、unique、updatable等。

@ColumnType：描述数据库字段类型，可对一些特殊类型作配置，进行特殊处理，主要属性有jdbcType、column、typeHandler等。

其他注解如：@Transient、@ColumnResult、@JoinColumn、@OrderBy、@Embeddable等暂不描述



5、 mapper中使用：单表操作，只需要继承 tk.mybatis 下的 Mapper 接口即可使用

import tk.mybatis.mapper.common.Mapper;

@Repository
public interface BrandMapper extends Mapper<Brand> {
}



6、 Service中使用：只需要依赖注入mapper然后调用方法

### 三、实体类定义

#### 注意事项：

​	1、属性类型是java类（Integer），不能是基础类（int）

#### 常用注解：

### 四、TKMyBatis中常用方法

#### 1、增

int insert(T var1)：增加一列

int insertSelective(T var1)：增加非空属性

#### 2、删

int deleteByPrimaryKey(Object var1)：根据主键删除

int delete(T var1)：根据非空属性值作为条件删除（参数实体中哪些字段不为null，就会被作为删除sql语句的条件字段，且条件关系是 and）

#### 3、改



#### 4、查

List<T> select(T var1);



SelectMapper 接口有一个方法 select，参数实体类中哪些字段不为 null，就会被作为 select sql 语句中的条件字段，且字段之间的关系是 and。

select 所有字段 from table where 字段1 = ？ and 字段2 = ？

```javascript
List<T> select(T var1);
```

复制

SelectOneMapper 接口有一个方法 selectOne，与 select 方法一样，只是返回结果只能为空或者一个，如果有多个，则抛出异常。

同上

```javascript
T selectOne(T var1);
```

复制

SelectCountMapper 接口有一个方法 selectCount，查询满足条件的记录有多少条。

select count(id) from table where 字段1 = ？ and 字段2 = ？

```javascript
int selectCount(T var1);
```

复制

SelectAllMapper 接口有一个方法 selectAll，查询全表所有记录。

select 所有字段 from table;

```javascript
List<T> selectAll();
```

复制

SelectByPrimaryKeyMapper 接口有一个方法 selectByPrimaryKey，根据主键进行查询。

select 所有字段 from table where 主键字段 = ？

```javascript
T selectByPrimaryKey(Object var1);
```

复制

ExistsWithPrimaryKeyMapper 接口有一个方法 existsWithPrimaryKey，根据主键查询某条记录是否存在。

select case when count(主键字段) > 0 then 1 else 0 end as result from table where 主键字段 = ?

```javascript
boolean existsWithPrimaryKey(Object var1);
```

复制

#### 3.4 修改

![tkmybatis详细教程（一篇就明白）](https://ask.qcloudimg.com/http-save/yehe-8223537/dc0fc59098f734658a1cb72de3e75edd.jpeg)

tkmybatis详细教程（一篇就明白）

UpdateByPrimaryKeyMapper 接口有一个方法 updateByPrimaryKey，根据主键字段准确地修改某一条记录。

update table set 所有字段

```javascript
int updateByPrimaryKey(T var1);
```

复制

UpdateByPrimaryKeySelectiveMapper 接口有一个方法 updateByPrimaryKeySelective，根据主键字段准确地修改某一条记录的部分字段（实体类参数的不为 null 的字段）。

update table set 部分字段

```javascript
int updateByPrimaryKeySelective(T var1);
```

复制

### 4 批量增删查改——基础方法

#### 4.1 批量插入

![tkmybatis详细教程（一篇就明白）](https://ask.qcloudimg.com/http-save/yehe-8223537/24cad6d9b316f0ef88707af6ca434db2.jpeg)

tkmybatis详细教程（一篇就明白）

这两个功能有一个要求，那就是操作的数据库表必须有一个自增主键，因为它要求主键必须要有一个默认值，否则就抛出异常。

这两个接口是集成到 MySqlMapper 接口中了，所以 dao 层的 mapper 接口还要继承 MySqlMapper 接口才能使用批量插入功能。

```javascript
public interface HouseMapper extends Mapper<House>, MySqlMapper<House> {
}
```

复制

InsertListMapper 接口有一个方法 insertList，批量插入。

insert into table (所有字段，除了自增主键) values (?,..,?), …,(?,…,?)

```javascript
int insertList(List<? extends T> var1);
```

复制

InsertUseGeneratedKeysMapper 接口有一个方法 insertUserGeneratedKeys，单个插入。

```javascript
int insertUseGeneratedKeys(T var1);
```

复制

#### 4.2 批量查询与批量删除

![tkmybatis详细教程（一篇就明白）](https://ask.qcloudimg.com/http-save/yehe-8223537/3b142b2050c93c270841934d6679aa2a.jpeg)

tkmybatis详细教程（一篇就明白）

SelectByIdsMapper 接口有一个方法 selectByIds，按照多个主键 id 值进行查询，但是方法的参数是 String，那么主键id之间用逗号隔开就行。

select 所有字段 from table where id in (id值1，id值2，…，id值n)

```javascript
List<T> selectByIds(String var1);
```

复制

DeleteByIdsMapper 接口有一个方法 deleteByIds，按照多个主键 id 值进行删除。

delete from table where id in (id值1，id值2，…，id值n)

```javascript
int deleteByIds(String var1);
```

复制

### 5 自定义查询条件

#### 5.1 删改查方法

图中接口都有一个共同点，就是需要 Example 对象作为方法的参数，Example 对象包含了我们各种自定义的查询条件，相当于 sql 语句中 where 部分的条件。

![tkmybatis详细教程（一篇就明白）](https://ask.qcloudimg.com/http-save/yehe-8223537/5072e116753dd0e1f2ce36a0c0c7c861.jpeg)

tkmybatis详细教程（一篇就明白）

![tkmybatis详细教程（一篇就明白）](https://ask.qcloudimg.com/http-save/yehe-8223537/c9b091f782a9991898a061c027d5b136.jpeg)

tkmybatis详细教程（一篇就明白）

每个接口都包含了一个方法，供我们调用。总结如下表：

| 方法                                                         | 功能描述                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| int deleteByExample(Object var1);                            | 一般参数就是Example对象，按照条件进行删除，返回删除的记录数  |
| List<T> selectByExample(Object var1);                        | 一般参数就是Example对象，按照条件进行查询，返回查询结果集    |
| int selectCountByExample(Object var1);                       | 一般参数就是Example对象，按照条件进行查询，返回符合查询条件的记录数 |
| T selectOneByExample(Object var1);                           | 一般参数就是Example对象，按照条件进行查询，结果只能为空或者一个，否则抛出异常 |
| int updateByExample(@Param(“record”) T var1, @Param(“example”) Object var2); | 第一个参数是新记录，第二参数是example对象，用新记录替换掉符合条件的旧记录 |
| int updateByExampleSelective(@Param(“record”) T var1, @Param(“example”) Object var2); | 功能同上，只是可以仅替换掉记录的部分字段                     |
| List<T> selectByRowBounds(T var1, RowBounds var2);           | 第一个参数是查询条件，第二个参数是 RowBounds 对象（包含2个属性，offset 和 limit），offset 表示起始行，limit 表示需要的记录数；方法的功能是按照查询条件进行查询，再按照 offset 和 limit 在结果集中取相应数量的记录。 |
| List<T> selectByExampleAndRowBounds(Object var1, RowBounds var2); | 第一个参数是 Example 对象，第二个参数是 RowBounds 对象，先根据 example 条件进行查询，再按照 offset 和 limit 取相应数量的记录。 |
| List<T> selectByConditionAndRowBounds(Object var1, RowBounds var2); | 同上                                                         |

#### 5.2 Example 条件设置

先创建 Example 对象，再创建 Example.criteria 对象，借助这两个对象，可以灵活地设置各种条件。Example 对象可以理解为 sql 语句层次的设置， 而 Example.criteria 对象可以理解为 sql 语句中的一个单一的条件表达式设置。

原理上可以理解为：一个 example 包含了若干个 criteria ，每个 criteria 就是 sql 语句中条件部分的一个括号部分（没有嵌套），比如 (id = 5)，criteria 包含了一个方法 void setAndOr(String andOr)，它的意思相当于在括号前面加上 and 还是 or，比如执行了方法 setAndOr(“and”)，那么 criteria 相当于 and (id = 5)，而 example 就把这些 criteria 拼凑起了，比如 example 包含了 2 个 criteria，分别是 (id = 5) 和 and (name = “张三”)，那么此 example 的效果就是 (id = 5) and (name = “张三”)。

```javascript
Example example = new Example(House.class);
Example.Criteria criteria = example.createCriteria();
```

复制

具体可以怎么设置呢？criteria 包含的方法总结如下表：

| 方法                                                         | 功能描述                                                     |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| andAllEqualTo(Object param)                                  | 所有字段都作为 where 后面的判断条件，判断值就是参数实体对象  |
| andBetween(String property, Object value1, Object value2)    | where property between value1 and value2 ，范围条件，包含两端 |
| andEqualTo(Object param)                                     | 实体对象中不为 null 的字段作为 where 后面的判断条件          |
| andEqualTo(String property, Object value)                    | 某一个<字段，值>作为 where 后面的判等条件                    |
| andGreaterThan(String property, Object value)                | 大于条件，某个字段大于某个值                                 |
| andGreaterThanOrEqualTo(String property, Object value)       | 大于等于条件，某个字段大于等于某个值                         |
| andIn(String property, Iterable values)                      | where property in ()，范围条件                               |
| andIsNotNull(String property)                                | where property is not null，判空条件                         |
| andIsNull(String property)                                   | where property is null，判空条件                             |
| andLessThan(String property, Object value)                   | 小于条件                                                     |
| andLessThanOrEqualTo(String property, Object value)          | 小于等于条件                                                 |
| andLike(String property, String value)                       | where property like value，注意 value 应该是一个匹配表达式   |
| andNotBetween(String property, Object value1, Object value2) | 范围条件，不包含两端                                         |
| andNotEqualTo(String property, Object value)                 | 要求字段不等于某个值                                         |
| andNotIn(String property, Iterable values)                   | 要求字段不在某个范围内                                       |
| andNotLike(String property, String value)                    | 模糊查询，要求不 like。                                      |
| void setAndOr(String andOr)                                  | 上面已经介绍过了                                             |

上表的方法都是“与”关系，即 and。 同样的，有相应的 “或” 关系，即 or。比如 orAllEqualTo、orGreaterThan 等等，都是将方法名中的 “and” 换成 “or”。

那 criteria 能否嵌套呢？能否有更方便的使用方式呢？回答：能，有。如下表：

| Example.Criteria orCondition(String condition, Object value) | condition参数是个sql字符串，可以拼接进 sql 语句的，value 是一个值，会拼接到 condition 后面的，此方法的最终结果为 or condition + value。 |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Example.Criteria orCondition(String condition)               | 功能同上，只是更加直接，一个字符串搞定，只是字符串参数可以写成类似这种 “id = ”+getId()，”( id = “+getId()+”)”，一样灵活，此方法的最终结果为 or condition。 |
| Example.Criteria andCondition(String condition)              | 不再赘述                                                     |
| Example.Criteria andCondition(String condition, Object value) | 不再赘述                                                     |

Example 类包含的方法总结如下表：

| 方法                                         | 功能描述                                                     |
| :------------------------------------------- | :----------------------------------------------------------- |
| void setDistinct(boolean distinct)           | 查询的结果是否要进行唯一性过滤，true表示过滤，false（默认）表示不过滤。 |
| void setOrderByClause(String orderByClause)  | 查询结果按照某个，或者某些字段进行升序，降序。比如参数是 “id asc” 就是按照 id 进行升序，“id asc，age desc” 就是按照 id 升序，在 id 相等的情况下，按照 age 降序。 |
| Example selectProperties(String… properties) | 当利用 example 进行查询时，此方法可以设置想要查询的字段是哪些，比如我只需要查询一张表的部分字段。 |
| Example.OrderBy orderBy(String property)     | 排序，与 setOrderByClause 功能一样，只是用法不同，比如 orderBy(“id”).asc() 表示按照 id 升序， orderBy(“id”).asc().orderBy(“age”).desc() 表示按照 id 升序，再按照 age 降序 |
| Example.Criteria or()                        | 创建一个 or 方式的、空的criteria，具体的 criteria 内容可以稍后设置。 |
| void or(Example.Criteria criteria)           | 直接以 or 的方式添加一个现有的 criteria                      |
| Example.Criteria and()                       | 同上，不过是 and 方式                                        |
| void and(Example.Criteria criteria)          | 同上，and 方式                                               |

