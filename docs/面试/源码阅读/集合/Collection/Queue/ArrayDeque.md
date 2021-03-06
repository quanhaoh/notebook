## ArrayDeque

### 重点

ArrayDeque的底层结构是循环数组，判空的条件是head = null && tai == null，判满的条件是heasd == tail

head指向数组的第一个元素

tail指向数组下一个要插入的位置，因此ArrayDeque始终有一个空位，ArrayDeque的添加元素的做法是添加

在对循环数组操作时，需要的注意的是：数组是否用完、数组下标越界

- 处理数组下标越界的做法：利用与运算的方式，对下标求模，例如，(head - 1) & (elementData.length - 1)、(tail- 1) & (elementData.length - 1)
- 将容量变为2的整数幂的做法：利用位运算求掩码，掩码加1即为大于该数的最小2的整数幂

由于循环数组的特殊性，在插入时，不需要进行判空处理

### 重要成员变量

```java
// 存储元素的数组
transient Object[] elements; // non-private to simplify nested class access

/**
 * The index of the element at the head of the deque (which is the
 * element that would be removed by remove() or pop()); or an
 * arbitrary number equal to tail if the deque is empty.
 */
transient int head;

/**
 * The index at which the next element would be added to the tail
 * of the deque (via addLast(E), add(E), or push(E)).
 */
transient int tail;

/**
 * 数组的最小容量，当使用带参构造方法初始化时，若指定大小小于8，则数组大小初始化为8
 */
private static final int MIN_INITIAL_CAPACITY = 8;
```

### 构造方法

ArrayDeque的扩容是先插入元素再尽心扩容，无参构造对象的容量在创建对象时就已经初始化好了

ArrayDeque双向数组的大小一定是2的整数次幂，最小是8，最大是2^30次方（int类型只有32位）

使用2的整数次幂是为了实现循环链表head和tail的运算正确，解决坐标越界的问题

```java
// 无参构造方法初始化
public ArrayDeque() {
    elements = new Object[16];
}

/**
 * 指定大小初始化，ArrayDeque会计算大于numElements的最小二次幂，再进行初始化
 * 若numElements小于8，则初始化数组的大小为8，这是最小的数组
 * @param numElements  lower bound on initial capacity of the deque
 */
public ArrayDeque(int numElements) {
    allocateElements(numElements);
}

// 初始化数组
private void allocateElements(int numElements) {
    elements = new Object[calculateSize(numElements)];
}

// 计算大于numElements的最小二次幂，并将其作为数组大小返回
private static int calculateSize(int numElements) {
  	// 初始化大小为8
    int initialCapacity = MIN_INITIAL_CAPACITY;
    // 计算大于numElements的最小二次幂 
    if (numElements >= initialCapacity) {
        initialCapacity = numElements;
        // 逻辑右移，并进行或运算，用于计算最小二次幂
        initialCapacity |= (initialCapacity >>>  1);
        initialCapacity |= (initialCapacity >>>  2);
        initialCapacity |= (initialCapacity >>>  4);
        initialCapacity |= (initialCapacity >>>  8);
        initialCapacity |= (initialCapacity >>> 16);
        // 加1，使initialCapacity变为2的整数次幂
        initialCapacity++;
		// 防止指定大小过大，当指定大小大于或等于2^30，则初始化大小改为2^30
        if (initialCapacity < 0)   // Too many elements, must back off
            initialCapacity >>>= 1;// Good luck allocating 2 ^ 30 elements
    }
    return initialCapacity;
}
```

### 自动扩容

ArrayDeque每次扩容的的新容量都是之前的2倍，以addFirst为例

```java
public void addFirst(E e) {
    // ArrayDeque不允许存入null值
    if (e == null)
        throw new NullPointerException();
    // 当head - 1 = -1时，(head - 1) & (elements.length - 1)即为求模运算，当head != 0时，结果即为head - 1
    elements[head = (head - 1) & (elements.length - 1)] = e;
    // 先插入元素后进行扩容
    if (head == tail)
        doubleCapacity();
}

// 双倍扩容
private void doubleCapacity() {
    // 断言head == tail，即数组已经用完，否则不进行扩容
    assert head == tail;
    int p = head;
    int n = elements.length;
    // 求出head到数组末尾间的元素个数
    int r = n - p; // number of elements to the right of p
    // 左移1位，乘2
    int newCapacity = n << 1;
    // 当旧数组的容量为2^30时，已经是最大容量，无法再扩容
    if (newCapacity < 0)
        throw new IllegalStateException("Sorry, deque too big");
    Object[] a = new Object[newCapacity];
    // 将head到数组末尾间的r个元素复制到新数组的[0, r)位置
    System.arraycopy(elements, p, a, 0, r);
    // 将数组始端到head的p个元素元素复制到新数组
    System.arraycopy(elements, 0, a, r, p);
    elements = a;
    // 复位head和tail
    head = 0;
    tail = n;
}
```

### addLast

```java
public void addLast(E e) {
    if (e == null)
        throw new NullPointerException();
    elements[tail] = e;
   // 防止坐标越界，并进行扩容判断
    if ( (tail = (tail + 1) & (elements.length - 1)) == head)
        doubleCapacity();
}
```

### poolFirst

```java
public E pollFirst() {
    int h = head;
    @SuppressWarnings("unchecked")
    E result = (E) elements[h];
    // 当数组为空时，返回null
    if (result == null)
        return null;
    elements[h] = null;     // Must null out slot
    // head后移一位
    head = (h + 1) & (elements.length - 1);
    return result;
}
```

### getLast

```java
public E getLast() {
    @SuppressWarnings("unchecked")
    E result = (E) elements[(tail - 1) & (elements.length - 1)];
    // 当数组为空时，抛出异常
    if (result == null)
        throw new NoSuchElementException();
    return result;
}
```

### 其他方法

<img src="https://gitee.com/quanhaoh/blogImage/raw/master/img/Queue和Deque接口的方法.png" style="zoom:;" />

