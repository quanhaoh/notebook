##  操作table：CRUD

### 1、创建：Create

**创建一张新表**

- `create table 表名(`

  `列名1 数据类型1 约束,`

  `列名2 数据类型2 约束,`

  `列名3 数据类型3 约束`,

  `primary key (列名)
  )engine = InnoDB;`

列的约束包括（每个约束之间用空格相隔）：

> 1、默认值约束 DEFAULT ~
>
> - DEFAULT 后面的值不允许取自函数，应尽量使用默认值而非 NULL 值
>
> 2、唯一性约束 UNIQUE(column)
>
> 3、非空约束 NOT NULL
>
> 4、自增约束 AUTO_INCREMENT
>
> - 每个表仅存在一个自增列，且该列必须被索引，大多用于主键列
>
> 5、主键约束 PRIMARY KEY
>
> - 主键不允许存在 NULL 值
> - 可用多个列的组合当主键，如 PRIMARY KEY (column1, column2)
>
> 6、外键约束 FOREIGN KEY

```mysql
# 创建学生信息表
create table student(
    id int,
    name varchar(1),
    age int,
    primary key(id)
    );
```

**复制一张表**

- `create table 表名 like 被复制的表名;`

```mysql
create table copy_studennt like student;
```

### 2、查询：Retrieve

查询所有数据库的名称

- `show tables;`

查询某张表的结构

- `desc 表名;`

```MySQL
desc student;
```

### 3、修改：Update

修改表名

- `alter table 表名 rename to 新表名;`

```mysql
alter table student rename to new_student;
alter table new_student rename to student;
```

修改表的字符集

- `alter table 表名 character set 字符集名;`

```mysql
alter table student character set utf8;
```

在某张表添加列

- `alter table 表名 add 列名 数据类型;`

```mysql
alter table student add sex varchar(1);
```

修改列的名称、类型

- `alter table 表名 change 列名 新列名 新数据类型;`
- `alter table 表名 modify 列名 新数据类型;`

```mysql
# 修改name列的名字为num，数据类型为int
alter table student change name mingzi int;
# 修改mingzi列的数据类型为varchar，长度为1
alter table student modify mingzi varchar(1);
# 修改mingzi列的名字为name
alter table student change mingzi name varchar(1);
```

删除列

- `alter table 表名 drop 列名;`

```mysql
alter table student add test date;
# 删除表student中的test列
alter table drop student;
```

### 4、删除表：Delete

删除某张表

- `drop table 表名;`

```MySQL
# 判断数据库是否存在，存在则删除
drop table if exists student;
```

