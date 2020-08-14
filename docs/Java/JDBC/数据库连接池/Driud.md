## Driud

### 1. 实现

#### 导入java包

#### 定义配置文件

名称：properties形式

可放在任意目录下

#### 创建数据库连接池对象：DruidDataSourceFactory

```java
Properties properties = new Properties();
InputStream input = Driud.class.getClassLoader().getResourceAsStream("druid.properties");
properties.load(input);
DataSource ds = DruidDataSourceFactory.createDataSource(properties);
```

#### 获取链接：getConnection()

```
Connection connection = ds.getConnection();
```

### 2.定义工具类

常用来加载配置文件，初始化连接池对象

- 获取连接池的方法

- 获取连接对象的方法：通过连接池获取连接对象

- 释放资源

  