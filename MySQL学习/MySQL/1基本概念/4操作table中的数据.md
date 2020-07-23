## 操作表中的数据：增删改

### 1、添加数据

#### 1.1 添加一行数据

- `insert into 表名(列名1, 列名2,列名3) values(值1, 值2, 值3);`

列和值要一一对应，不一定要插入每个列的值，也可省略一些列，省略的列会默认为 NULL 值

```mysql
insert into student(id, name, age, sex) values(1, "1号", 20, "男");
```

### 1.2 添加多行数据

方法1：多次使用 insert 语句 
方法2：使用 values子句的多个值，以逗号相隔

- `insert into 表名(列名1, 列名2,列名3) values(值1, 值2, 值3), (值1, 值2, 值3);`

```mysql
insert into student(id, name, age, sex) values(2, "2号", 20, "男")，(3, "3号", 21, "女");
```

### 1.3 添加检索出的数据

同时使用insert 和 select 语句，用 select 语句代替 values 子句

- `insert into 表名(列名1 列名2)  select 列名1，列名2 from 表名;`

```mysql
# 从另一个学生表中查询数据并插入新的表
insert into student(id, name, age, sex)
select id, name, age, sex from another_student;
```

### 2、删除数据

#### 2.1 删除一行数据

- `delete from 表名 where 列名=值;`

 ```mysql
delete from student where id=3;
 ```

#### 2.2 删除所有数据

方法1：`delete from 表名;`

- 不推荐使用，因为会一行行地删除，效率低

方法2：`truncate table 表名;`

- 推荐使用，先删除表，再创建一张一样地空表

```mysql
delete from student;
truncate table student;
```

### 3、修改数据

#### 3.1 修改某行数据某列的值

筛选条件中的列==必须是主键==

- `update 表名 set 列名1=值1， 列名2=值2 where 筛选条件;`

```mysql
# 修改一行数据
update student set sex="女" where id=2;
# 同时修改多行数据
update student set age=16 where id=1, id=2;
```

#### 3.2 同时修改所有行某列的值

- `update 表名 set 列名1=值1， 列名2=值2;`

```mysql
update student set ...;
```

