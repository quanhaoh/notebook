jdk9添加的特性

### 注意点

- of方法适用于List、Set、Map接口，不适用于接口的实现类

- of方法的返回值是一个不可变的集合，不能再使用add、put等方法修改集合

- Set、Map接口在调用of'方法时，不能添加重复元素

```Java
List<String> list = List.of("a", "b");
Set<Integer> set = Set.of(1,2,3,4);
Map<String, Integer> map = Map.of("a", 1, "b", 2, "c", 3 );
```

