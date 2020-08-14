## 使用 Lambda 表达式遍历集合

Iterable 接口存在 forEach(Consumer action) 默认方法，参数类型是函数式接口

Iterable 接口为 Collection 接口的父接口，故 Collectiion 集合可直接调用该方法

当调用 forEach(Consumer action) 时，程序依次将集合元素传给 Consumer 的 accept(T t) 方法

即使用 Lambda 表达式来实现 Consumer 接口的抽象方法

```Java
Collection books = new HashSet();
books.add("三国");
books.forEach(obj -> System.out.println("迭代集合元素：" + obj);
```

## 使用 Iterator 遍历集合元素

Iterator 用于遍历 Collection 集合中的元素，即 Iterator 对象称为迭代器

Iteartor 接口含有四个方法：

方法 | 说明
---|---
boolean hasNext() | 若被迭代的集合元素未遍历完 ，则返回 true
Object next() | 返回集合中的下一个元素
Object remove() | 删除集合中上一次 next 方法返回的元素
void forEachRemaining(Consumer action) | 使用 Lambda 表达式遍历集合元素


```Java
Collection books = new HasSet();
...
Iterator it = books.iterator();
while(it.hasNext()){
    String book = (String)it.next();
    System.out.println(book);
}
```

Iterator 对象必须关联一个 Collection 对象

对集合元素迭代时，仅是把元素的值赋给迭代遍历（book），修改迭代变量不会对集合元素本身造成影响

**迭代访问集合元素的过程中不能改变集合元素**

## 使用 Lambda 表达式遍历 Iterator


```Java
Collection books = new HasSet();
...
Iterator it = books.iterator();
it.forEachrRemaining(obj -> System.out.println("迭代集合元素：" + obj);
```

## 使用 foreach 循环遍历集合元素

```Java
for(Object obj:books){
    String book = (String)obj;
    ...
}
```
foreach 循环的迭代遍历也只是存着集合元素的值，对其改变不影响集合元素
