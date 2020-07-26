## ArrayList

### 重点

ArrayList的实现基于动态数组，最常见的操作是扩容、数组复制

ArrayList可以随机访问，但扩容、数组复制的资源消耗较大，比较适用于读多写少的情况

### 重要成员变量

```java
// 序列化uuid
private static final long serialVersionUID = 8683452581122892189L;

// ArrayList默认初始化大小（不是在创建对象时使用，而是在第一次add元素时扩容大小为10）
private static final int DEFAULT_CAPACITY = 10;

// 调用带参构造方法，且初始化容量为0时，elementData即初始化为该空数组
private static final Object[] EMPTY_ELEMENTDATA = {};

// 调用无参构造方法时，elementData即初始化为该空数组
// 扩容时用于判断对象是否为无参构造方法创建的，是则将数组扩容为10
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

// 元素数组，ArrayList所有的操作都是基于该动态数组进行的
transient Object[] elementData; // non-private to simplify nested class access

// 表示当前列表中的元素个数
private int size;
```

### 构造方法

```java
/**
 * 创建指定容量的ArrayList对象，初始化elementData为指定容量
 * @param  initialCapacity  the initial capacity of the list
 * @throws IllegalArgumentException if the specified initial capacity
 *         is negative
 */
public ArrayList(int initialCapacity) {
    // 做参数检验
    if (initialCapacity > 0) {
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        throw new IllegalArgumentException("Illegal Capacity: "+
                                           initialCapacity);
    }
}

/**
 * 创建一个空elementData，当第一个add元素时扩容至10
 */
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}

/**
 * 将指定集合的数组复制到相同大小的elementData中
 * @param c the collection whose elements are to be placed into this list
 * @throws NullPointerException if the specified collection is null
 */
public ArrayList(Collection<? extends E> c) {
    // 将集合转换为Object数组，toArray方法本质上是调用Arrays.copyOf()方法
    Object[] a = c.toArray();
    if ((size = a.length) != 0) {
        // 将数组复制到elementData
        if (c.getClass() == ArrayList.class) {
            elementData = a;
        } else {
            elementData = Arrays.copyOf(a, size, Object[].class);
        }
    } else {
        // replace with empty array.
        elementData = EMPTY_ELEMENTDATA;
    }
}
```

### 自动扩容：add、addAll

add方法中没有对添加的元素进行类型检验，因为elementData是Object类型的，可以添加任意类型元素，包括null值

add(E e)

- 顺序插入，直接插入elementData末尾

add(int index, E e)

- 在指定的index插入，比直接插入多出一个移动元素的过程（源码实现上是复制数组）

```java
/**
 * 在列表末尾插入元素
 * @return <tt>true</tt> (as specified by {@link Collection#add})
 */
public boolean add(E e) {
    // 扩容检查,需要才进行扩容
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    elementData[size++] = e;
    return true;
}

/**
 * 在指定index插入元素
 * @throws IndexOutOfBoundsException {@inheritDoc}
 */
public void add(int index, E element) {
    // 检测index是否在数组范围内
    rangeCheckForAdd(index);
    // size + 1为默认最小扩容量，刚好满足需求
    // 如果是addAll方法，则最小扩容量为size + 插入元素数量
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    // 将index后面的元素后移1位
    System.arraycopy(elementData, index, elementData, index + 1,
                     size - index);
    elementData[index] = element;
    size++;
}
```

```java
private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

// 使用无参构造方法创建列表时，elementData即为DEFAULTCAPACITY_EMPTY_ELEMENTDATA空数组
private static int calculateCapacity(Object[] elementData, int minCapacity) {
    // 设置扩容量为10（所谓的初始化大小为10是直到这里才实现）
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}

private void ensureExplicitCapacity(int minCapacity) {
    // 列表修改次数加1
    modCount++;

    // 只有当列表要用完时，才会扩容，否则跳过
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}

private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
    	// 设置新容量为旧容量的1.5倍
        int newCapacity = oldCapacity + (oldCapacity >> 1);
    	// 如果1.5倍的扩容量小于最小扩容量，则直接扩容到最小扩容量
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
    	// 比较新容量和Integer最大值，最大只能扩容至Integer的最大值
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // 扩容操作，即复制到指定大小的数组中
        elementData = Arrays.copyOf(elementData, newCapacity);
}

private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow
        throw new OutOfMemoryError();
    return (minCapacity > MAX_ARRAY_SIZE) ?
        Integer.MAX_VALUE :
    MAX_ARRAY_SIZE;
}
```

### 迭代器：Interator

在ArrayList定义了内部类Itr，实现了迭代器，维护了指向elementData的索引cursor、lastRet

当遍历过程中列表结构被改变，索引并不会跟着改变，所以会抛出ConcurrentModificationException

##### 重要成员变量

```java
int cursor;       // 指向下一个要返回的元素
int lastRet = -1; // 指向上一个返回的元素
int expectedModCount = modCount;// 用于fail-fast机制，当二者不相等时，列表结构被修改，则抛出异常
```

##### 获取元素

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
    // 将cursor指向下一个元素
    cursor = i + 1;
    // 返回当前元素
    return (E) elementData[lastRet = i];
}
// fial-fast检测，当列表结构被修改（插入、删除）时，抛出异常
final void checkForComodification() {
    if (modCount != expectedModCount)
        throw new ConcurrentModificationException();
}
```

##### 安全删除元素：不会触发fail-fast机制

```java
public void remove() {
    if (lastRet < 0)
        throw new IllegalStateException();
    checkForComodification();

    try {
        ArrayList.this.remove(lastRet);
        // cursor、lastRet同步减1
        cursor = lastRet;
        lastRet = -1;
        // 更新expectMoCount，防止触发fail-fast机制
        expectedModCount = modCount;
    } catch (IndexOutOfBoundsException ex) {
        throw new ConcurrentModificationException();
    }
}
```

### 删除元素：remove、removeRange

删除元素的原理和在add(index, element)的原理相同，都是移动元素（复制数组），时间复杂度都为O(n)

removeObject o)

- 删除指定元素，遍历elementData，删除第一个与之相等的元素，挪动数组


remove(int index)

- 删除指定位置元素

removeRange(int fromIndex, int toIndex)

- 删除[fromIndex, toIndex - 1]间的元素

```java
public E remove(int index) {
    rangeCheck(index);

    modCount++;
    E oldValue = elementData(index);
	// 挪动数组即为删除，numMoved为需要挪动的元素个数
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    // 将该位置赋为null，以便GC回收对象
    elementData[--size] = null; // clear to let GC do its work

    return oldValue;
}
```

### 序列化

### 其他常用方法

| 方法                                     | 说明                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| set(Object , int index)                  | 直接修改指定位置的元素                                       |
| indexOf(Object o)、lastIndexOf(Object o) | 正序遍历获取第一个匹配的元素位置、<br />倒序遍历获取最后一个匹配的元素位置 |
| contains(Object o)                       | return indexyOf(o) > 0                                       |
| isEmpty()                                | rreturn size == 0                                            |
| size()                                   | reyurn size                                                  |
| sort()                                   | 调用Arrays.sort()进行快速排序                                |
| subList(int fromIndex, int toIndex)      | 返回[fromIndex, toIndex - 1]的定长子列表，内部定义SubList类<br />效果和实现原理与Arrays的asList()类似 |
| clone()                                  | 拷贝一个内容相同的新对象，先调用Object.clone()方法创建对象，再调用Arrays.copyOf()复制数组 |
| clear()                                  | 清空列表，即遍历列表，为elmentData每个位置赋null             |
