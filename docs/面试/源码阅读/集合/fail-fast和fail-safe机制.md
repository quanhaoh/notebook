## fail-fast机制

> A fail-fast system is nothing but immediately report any failure that
> is likely to lead to failure. When a problem occurs, a fail-fast system
> fails immediately. In Java, we can find this behavior with iterators.
> Incase, you have called iterator on a collection object, and another
> thread tries to modify the collection object, then concurrent modification
> exception will be thrown. This is called fail-fast

简单理解为当遍历集合时，若集合结构被修改（删除、添加）时，则会直接抛 ConcurrentModificationException

### 场景

- 迭代器遍历、foreach遍历都有fail-fast机制
- 但for循环遍历没有实现fial-fast机制，但也会出现数据错误的情况

### 适用的集合

List

- ArrayList
- LinkedList
- Vector

Set

- HashSet
- LinkedHashSet
- TreeSet

Queue

- 

Map

- HashMap
- LinkedHashMap
- TreeMap
- HashTable

### 实现原理

通过记录modCount参数来实现，集合的每次==添加、删除==操作modCount都会加1

Inetrator类维护着expectedModCount变量

初始化迭代器时，expectedModCount = modCount，遍历过程中当二者不相等时即列表结构被改变

```java
// 获取下一个元素
@SuppressWarnings("unchecked")
public E next() {
    checkForComodification();
    int i = cursor;
    if (i >= size)
        throw new NoSuchElementException();
    Object[] elementData = ArrayList.this.elementData;
    if (i >= elementData.length)
        throw new ConcurrentModificationException();
    cursor = i + 1;
    return (E) elementData[lastRet = i];
}
// fial-fast检测，当列表结构被修改（插入、删除）时，抛出异常
final void checkForComodification() {
    if (modCount != expectedModCount)
        throw new ConcurrentModificationException();
}
```

```java
// 获取下一个元素
@SuppressWarnings("unchecked")
public E next() {
    checkForComodification();
    int i = cursor;
    if (i >= size)
        throw new NoSuchElementException();
    Object[] elementData = ArrayList.this.elementData;
    if (i >= elementData.length)
        throw new ConcurrentModificationException();
    cursor = i + 1;
    return (E) elementData[lastRet = i];
}
// fial-fast检测，当列表结构被修改（插入、删除）时，抛出异常
final void checkForComodification() {
    if (modCount != expectedModCount)
        throw new ConcurrentModificationException();
}
```

### 解决方案

#### 1、使用Collections#synchronizedList

synchronizedList为每个方法都添加sychronized锁

#### 2、使用并发包CopyOnWriteArrayList来解决

#### 3、 使用iterator的remove方法来删除元素

该方法会同步更新iterator的索引

#### 4、fail-safe机制

解决了遍历过程中集合结构被修改的问题

在初始化时创建一个集合副本，在副本上进行遍历，原集合的修改不会影响到副本的遍历

#### 弊端

当原集合被修改时，遍历的数据不是最新的

需要消耗更多的资源来创建副本