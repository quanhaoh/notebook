

1. 定义数据库table同名类
2. 创建封装方法，连接数据库，读取table
3. 调用封装方法
4. 返回同名类对象集合



```java
public static void main(String[] args) throws SQLException, ClassNotFoundException {
        Connection connection = JDBCUtils.connection();
        Statement statement = connection.createStatement();
        String sql = "select * from student";
        ResultSet result = statement.executeQuery(sql);
        List<Student> list = getStudent(result);
        for (Student student:list){
            System.out.println("学生信息~");
            System.out.println("\t姓名:" + student.getName());
            System.out.println("\t学号：" + student.getId());
            System.out.println("\t性别：" + student.getSex());
            System.out.println("\t年龄：" + student.getAge());
        }

    }

    /**
     * 遍历所读取的数据库，将每一行数据封装为Student对象
     * @param result
     * @return List<Student>
     * @throws SQLException
     */
    public static List<Student> getStudent(ResultSet result) throws SQLException {
        List<jdbc_mysql.Student> list = new ArrayList<>();
        jdbc_mysql.Student student = new jdbc_mysql.Student();
        while(result.next()){
            student.setId(result.getInt("id"));
            student.setName(result.getString("name"));
            student.setAge(result.getInt("age"));
            student.setSex(result.getString("sex"));
            list.add(student);
        }
        return list;
    }
```



