## List 集合

**元素有序、可重复的集合，类似于数组，有元素索引**

List 作为 Collection 接口的子接口，除了可以使用 Collection 接口的全部方法，还有一些根据索引操作集合元素的方法

### 1、常用方法

- add(index, element)：在某位置添加元素，后面的自动往后移一位
- remove(index)
- get(index)
- set(index, element)：替换某位置的元素

包括了两个默认方法
- void replaceAll(UnaryOperator operator)：根据 operator 的计算规则重设集合中的所有元素值
- void sort(Comparator c)：根据 Comparator 参数对集合元素排序

Comparator、UnaryOperator 都是函数式接口，都可用 Lambda 表达式作为参数

```Java
List books = new ArrayList();
books.add(1, "三国");
books.remove(0);
books.sort((var1, var2) -> ((String)var1.length() - (String)var2.length()));
books.replaceAll(var -> ((String)var.length());
```

#### listIterator() 方法
该方法返回一个 ListIterartor 对象，ListIterartor 接口继承了 Iterator 接口，提供了操作 List 的方法

添加的方法
- boolean hasPrevious()：判断集合中是否还有上一个元素
- Object previous()：返回迭代器的上一个元素
- void add(Object)：在指定位置插入元素

ListIterator 增加了**向前迭代**、**添加元素**的功能，Iterator 只能向后迭代且只能删除元素而不能增加

**迭代集合时，需先正向迭代，才可进行方向迭代**

```Java
List books = new ArrayList();
ListIterator book = books.listIterator();
while(book.hasNext()){
    book.add("分割线");
}
while(book.hasPrevious()){
    ...
}
```

### 2、ArrayList 和 Vector 实现类

二者是 List 类的典型实现类，**基于数组实现**，因此ArrayList**查询快、增删慢**，每添加一个元素都要重新创建数组并复制元素值

二者封装了一个动态、可再分配的 Object[] 数组，其对象通过 initial、Cpacity 参数来设置数组长度，当数组溢出时则自动增加

- ArrayList 类是非线程安全的,Vector 类是线程安全的，但不建议使用 Vector 类
- Vector 类性能较差，非常古老

### 3、LinkedList类

基于链表实现，查询慢、增删快

使用LindkedList特有的方法，不能使用多态

- add(E e)：在集合末尾添加元素
- addFirst(E e)：在集合头部添加元素
- addLast(E e)
- getFirst()：获取集合第一个元素
- getLast()
- clear()：清空集合中的元素，要先判断集合是否为空
- removeFirst()：移除集合第一个元素
- removeLast()

#### 固定长度的 List

操作数组的工具类：Arrays 类

该类提供了 asList(Object... a) 方法，可将一个数组或指定个数的对象转换为 List 集合，该集合是 Arrays 的内部类 ArrayList 的实例

Arrays.ArrayList 是固定长度的 List 集合，只能遍历，不可增加、删除


```Java
List books = Arrays.asList("三国"，“水浒”);
```
