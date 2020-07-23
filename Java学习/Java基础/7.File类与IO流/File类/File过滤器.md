## File过滤器

File类中包含两个重载ListFiles的方法，传递的参数时过滤器

### FileFilter接口

用于过滤File对象

唯一抽象方法：

- `boolean accept(File pathName)`：测试File对象是否应该包含再某个路径名列表中
  - pathName，即使用listFiles方法遍历目录，获取的每一个File对象
  - 自定义过滤规则，若符合条件的File对象则返回true，否则返回false

```Java
File dir = new File("...");
File[] files = dir.listFiles(new FileFilter(){
    @Override
    public boolean accept(File file) {
        return ...
    }
});
```

#### 原理

- listFiles(...)方法遍历dir中的子目录和文件，封装为File对象
- 传递封装好的File对象到accept函数，若满足过滤要求则添加至files数组中，否则丢弃

### FileNameFilter接口

用于过滤文件名称

唯一抽象方法：

- `boolean accept(File dir, String name)`：测试File对象是否应该包含在某个路径名列表中
  - dir，即创建File对象时传递的目录
  - name，即使用listFiles方法遍历目录，获取的每一个Files对象对应的名称

```Java
File dir = new File("...");
File[] files = dir.listFiles((File dir, String name)->{
    return ...
});
```

