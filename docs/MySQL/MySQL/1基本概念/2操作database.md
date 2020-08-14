## 操作database：CRUD

### 1、创建：Create

创建新数据

- `create database 数据库名;`

```MySQL
# 创建数据库db1
create database db1;
# 先判断数据库db1是否存在，存在则创建
create database if not exists db1;
# 创建数据库db1，设置数据库的字符集
create database chracter gbk set;
# 先判断数据库db1是否存在，存在则创建，且设置数据库的字符集
create database if not exists db1 chracter gbk set;
```

### 2、查询：Retrieve

查询所有数据库的名称

- `show databases;`

查询某个数据库的创建语句

- `show create database 数据库名;`

```MySQL
show create database db1;
```

### 3、修改：Update

修改数据库的字符集

- `alter database 数据库名 character set 字符集类型名;`

```mysql
alter database db1 character set utf8;
```

### 4、删除：Delete

删除数据库

- `drop database 数据库名;`

```MySQL
# 判断数据库是否存在，存在则删除
drop database if exists db1;
```

#### 5、使用数据库

查询当前正在使用的数据库名称

- `select database();`

使用某数据库

- `use 数据库名;`

```mysql
use db1;
```