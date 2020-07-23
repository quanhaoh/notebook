## JdbcTemplate

![image](https://gitee.com/quanhaoh/blogImage/raw/master/img/JdbcTemplate的框架图.png)

JdbcTemplate的作用封装与数据库的交互，实现CRUD的操作

#### 常用方法

update()

query()

```java
DriverManagerDataSource dataSource = new DriverManagerDataSource();
dataSource.setDriverClassName("com.jdbc.mysql.Driver");
dataSource.setUrl("jdbc:mysql//localhost:3306/..");
dataSource.setUsername("root");
dataSource.setPassword("password");

JdbcTemplate template = new JdbcTemplate(dataSrouce);
template.setDataSource(dataSource);
template.query("select * from table where id = ?", 1);
template.update("delete from table where id = 1");
```

#### Spring内置的数据源模块：DriverManagerDataSource

#### 多个Dao类重复定义JdbcTemplate的问题

当存在多个Dao使用JdbcTemplate时，需要重复定义JdbcTemplate对象并实现依赖注入，代码重复过多

解决办法：

- 方法1：继承Spring内置的JdbcDaoSupport类，直接使用JdbcTemplate对象
- 方法2：或者自己抽取重复代码，封装实现一个JdbcDaoSupport类用于继承使用
- 方法1的弊端是可以直接使用而不能使用Ioc依赖注入，方法2则可以自定义并使用依赖注入

#### Spring包含许多数据库相关的操作模板类

操作关系型数据库

- JdbcTemplate
- HiberbateTemplate

操作nosql数据库

- RedisTemplate

操作消息队列

- JmsTemplate