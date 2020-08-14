## LinkedList

LinkedList底层是双向链表，同时还具备stack和queue的一些方法，可以当成stack和queue使用

ArrayQueue作为队列或栈的性能要好于LinkedList

LinkedList针对头尾节点的CRUD效率很高，其他情况的操作基本都建立在遍历寻找节点的基础上，时间复杂度都为O(n)

相较于ArrayList的插入和删除的效率，LinkedList的效率相对高一些，更适用于写多读少的情况

### 重点

LinkedList的实现基于双向链表，CRUD功能都在链表的基础上进行

LinkedList的设计思想就是把链表遍历求节点、插入、删除的代码封装起来，私有化

公有方法实现参数的检验、边界条件的判断，涉及到链表操作才调用这些私有方法来实现数据操作

```java
// 遍历查找特定index的元素
Node<E> node(int index) {
    // 由于双向链表的特殊性，将index与中点比较后，决定从first还是last开始遍历，缩减一半的量
    if (index < (size >> 1)) {
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
    
// 插入头节点
private void linkFirst(E e);
// 插入尾节点
void linkLast(E e) {
    final Node<E> l = last;
    final Node<E> newNode = new Node<>(l, e, null);
    last = newNode;
    if (l == null)
        first = newNode;
    else
        l.next = newNode;
    size++;
    modCount++;
}
// 在特定节点前插入元素
void linkBefore(E e, Node<E> succ) {
    // assert succ != null;
    final Node<E> pred = succ.prev;
    final Node<E> newNode = new Node<>(pred, e, succ);
    succ.prev = newNode;
    if (pred == null)
        first = newNode;
    else
        pred.next = newNode;
    size++;
    modCount++;
}

// 删除元素
private E unlinkFirst(Node<E> f)
private E unlinkLast(Node<E> l)
E unlink(Node<E> x
```

### 重要成员变量

```java
// 链表中节点的个数
transient int size = 0;

/**
 * 头结点
 * Invariant: (first == null && last == null) ||
 *            (first.prev == null && first.item != null)
 */
transient Node<E> first;

/**
 * 尾结点
 * Invariant: (first == null && last == null) ||
 *            (last.next == null && last.item != null)
 */
transient Node<E> last;

// 内部节点类Node的构成
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

### 构造方法

LinkedList()

LinkedList(Collection<? extends E> c)

```java
public LinkedList(Collection<? extends E> c) {
    this();
    addAll(c);
}
```

### add、addAll、addFirst、addLast

链表插入元素要考虑的情况：空链表插入、插入尾节点、插入头节点、插入普通位置节点

尾节点插入和特定节点插入的思想是一致的：

- 获取pre、判断是否头节点（pre == null）、头节点插入/正常插入
- 区别在与尾节点插入是已知pre的，而特定节点插入需要遍历出该节点，再求pre

 add(E e)

- 在尾部插入节点

 add(int index, E e)

- 在指定位置插入节点

addAll(Collection<? extends E> c)

- 在尾部插入系列元素，调用下面的方法

addAll(int index, Collection<? extends E> c)

- 在指定位置插入系列元素，与普通插入原理一致，获取c.toArray后，循环插入

```java
/**
 * Appends the specified element to the end of this list.
 * <p>This method is equivalent to {@link #addLast}.
 * @param e element to be appended to this list
 * @return {@code true} (as specified by {@link Collection#add})
 */
public boolean add(E e) {
    linkLast(e);
    return true;
}
```

```java
public void add(int index, E element) {
    // 参数检验，判断index是否合法
    checkPositionIndex(index);
	// 边界条件，直接插入尾节点
    if (index == size)
        linkLast(element);
    else
        linkBefore(element, node(index));
}
```

### getFirst、getLast

- getFirst()，判空后返回first
- getLast()，判空后返回last

### set

set(E e)，替换尾结点元素

set(int index, E e)，替换特定位置的元素

```java
public void set(E e) {
    if (lastReturned == null)
        throw new IllegalStateException();
    checkForComodification();
    lastReturned.item = e;
}

public E set(int index, E element) {
    checkElementIndex(index);
    // 遍历查找该节点
    Node<E> x = node(index);
    E oldVal = x.item;
    x.item = element;
    return oldVal;
}
```

### remove、removeFirst、removeLast

链表删除元素要考虑的情况：空链表判断、删除尾节点、删除头节点、删除后要赋null值以便GC

remove()，删除头节点

remove(Object o)，删除指定节点

```java
public E remove() {
    return removeFirst();
}
```

```java
public boolean remove(Object o) {
    // 遍历链表，删除第一个null值
    if (o == null) {
        for (Node<E> x = first; x != null; x = x.next) {
            if (x.item == null) {
                unlink(x);
                return true;
            }
        }
    } else {	// 遍历链表，删除第一个与之相等的节点
        for (Node<E> x = first; x != null; x = x.next) {
            if (o.equals(x.item)) {
                unlink(x);
                return true;
            }
        }
    }
    return false;
}
```

### 常用方法

和ArrayList中的常用方法几乎一致

### 队列和栈方法

由于双向链表的特殊性，ArrayList维护着first、last节点，在已实现的add、remove方法下，实现了队列和栈的各种特性方法

#### Queue方法

| 方法            | 说明                                  |
| --------------- | ------------------------------------- |
| peak()          | 读取队列头元素，返回first             |
| element()       | 读取队列头元素，返回getFirst()        |
| poll()          | 删除队头元素并返回，调用unLinkFirst() |
| remove          | 删除队头元素并返回，调用removeFirst() |
| offer(Object o) | 往队尾插入元素，调用add(o)            |

#### Dequeue方法

| 方法            | 说明                                  |
| --------------- | ------------------------------------- |
| peakFirst()     | 读取队列头元素，返回first             |
| peakLast()      | 读取队列尾元素，返回last              |
| polFirst()      | 删除队头元素并返回，调用unLinkFirst() |
| pollLast()      | 删除队尾元素并返回，调用unLinkLast()  |
| offerFirst(E e) | 往队头插入元素，调用addFirst(e)       |
| offerLast(E e)  | 往队尾插入元素，调用addLast(e)        |

#### Stack方法

| 方法      | 说明                            |
| --------- | ------------------------------- |
| pop()     | 弹出栈头元素，调用removeFirst() |
| push(E e) | 压入栈，调用addFirst(e)         |

