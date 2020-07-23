## Set 集合

### 常用方法

### 1、HashSet 实现类

按照哈希算法来存储集合中的元素，底层是**哈希表结构**，存取、查找性能好

- 无序，即取得顺序不一定时存的顺序
- 不可重复，存储自定义元素`必须重写`hashCode()方法和equals()方法，否则无法保证不重复
- 不能同步访问，当多线程访问同一个 HashSet 集合时，需通过diamagnetic保证同步
- 集合元素值可以是  null

**添加集合元素时，会自动通过该对象的 hasCode() 计算哈希值，若哈希值重复则用equals() 判断两者的值是否相同**

> 重写该类对象的两个方法时，应保持同步，相等的对象 hasCode() 返回的哈希值应相等

```Java
HashSet books = new HashSet();
books.add(new A());
...
```

### 2、LinkedHashSet 类

- HashSet 类的子类
- 底层也是哈希表结构，但会使用链表维护元素的次序，即按添加顺序保存元素
- 有序，即存取顺序相同
- 不可重复

性能略低，适用于迭代遍历集合元素

### 3、TreeSet 类

SortedSet 接口的实现类，集合中的元素是有序的，根据元素实际值大小进行排序

TreeSet 采用红黑树存储集合元素

#### 自然排序

通过调用集合元素的 compareTo(Object obj) 方法比较元素大小，并按照升序排列

Comparable 接口定义了 compareTo(Object obj) 方法，根据两个对象的大小返回整数值，相等则返回0，大于则返回1，小于则返回1

**实现了 comparable 接口的常用类**

header 1 | header 2
---|---
BigDecimal、BigInteger 以及所有数值型对应的包装类 | 按值进行比较
Character 类 | 按照 UNICODE 值进行比较
Boolean 类 | true 大于 false
Stringr 类 | 按照 UNICODE 值进行比较
Date、Time 类 | 时间、日期越后越大

TreeSet 中尽量存放同类型对象，因为进行比较时会将对象强制转化为同类型比较，若不能转换则会抛出异常


```
TreeSet books = new TreeSet();
books.add(new A());
...
```

#### 定制排序

创建 TreeSet 集合对象时，提供一个与 Comparator 对象与之关联，则集合元素的排序规则可由其决定

Comparable 未函数式接口，故用 Lambda 表达式代替该对象


```
TreeSet books = new TreeSet((var1, var2) -> 
    {
        A a1 = (A)var1;
        A a2 = (A)var2;
        return A1.num > A2.num ? -1:A1.num < A2.num ? 2:0;
    });
    books.add(new A());
    ...
```

### 4、EnumSet 类

EnumSet 集合对象所有的元素必须是指定枚举类型的枚举值，根据枚举值在枚举类中的定义顺序决定排序

- 不允许添加 null 值
- 通过类名来创建枚举类实例



```
// 创建空集合，指定对象的枚举类未 Season 类
EnumSet seasons = EnumSet.noneOf(Season.class);
seasons.add(Season.SPRING);
...
```
