## xml和注解配置

- configuration
  - properties（属性）
  - settings（设置）
  - typeAliases（类型别名）
  - typeHandles（类型处理器）
  - objectFactory（对象工厂）
  - plugins（插件）
  - environments（环境配置）
    - environment（环境变量）
      - transactionManager（事务管理器）
      - dataSource（数据源）
  - databaseIdProvider（数据库厂商标识）
  - mappers（映射器）

实质上是使用dom4j来解析xml

### xml配置

主配置文件中mapper标签使用resource属性

```xml
<mapper resource="mapper接口路径">
```

实现映射xml配置文件

```xml
<select id="方法名" resultType="数据库表对应类的路径">
```

### 注解配置

主配置文件中mapper标签使用class属性

```xml
<mapper class="mapper全限定类名">
```

在mapper接口的方法前使用@select(sql语句)