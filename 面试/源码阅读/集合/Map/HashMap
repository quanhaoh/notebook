## HashMap

### 重点

HashMap没有修改键值的方法，是因为put方法会覆盖相同key的value值，相当于修改

### 重要成员变量

```java
// 默认初始化容量，即16
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16

// 最大容量，即2^30，即int类型所能表示的最大2的整数幂
static final int MAXIMUM_CAPACITY = 1 << 30;

// 默认装载因子，即0.75，用于进行扩容时候的判断
static final float DEFAULT_LOAD_FACTOR = 0.75f;

// 当桶中的链表长度等于8时，链表会转换为红黑树
static final int TREEIFY_THRESHOLD = 8;

// 当红黑树节点数小于6时，红黑树会转换为链表
static final int UNTREEIFY_THRESHOLD = 6;

static final int MIN_TREEIFY_CAPACITY = 64;

// 散列槽数组，长度为capacity
transient Node<K,V>[] table;

// entrySte()方法的缓存
transient Set<Map.Entry<K,V>> entrySet;

// 散列表中元素个数
transient int size;

// 散列表修改次数，用于fial-fast机制
transient int modCount;

// 值为capacity * loadFactor，即扩容临界点，当size == threshold时即自动扩容
int threshold;

// 装载因子，可以进行修改，默认值为0.75
final float loadFactor;
```

### 构造方法

```java
// 自定义容量和装载因子
public HashMap(int initialCapacity, float loadFactor) {
	...	// 对两个参数进行有效性i=检查
    this.loadFactor = loadFactor;
    this.threshold = tableSizeFor(initialCapacity);
}

// 自定义容量，装载因子为0.75
public HashMap(int initialCapacity) {
    this(initialCapacity, DEFAULT_LOAD_FACTOR);
}

// 默认构造，容量为16，装载因子为0.75
public HashMap() {
    this.loadFactor = DEFAULT_LOAD_FACTOR; 
}

// 基于原map的基础上创建新map
public HashMap(Map<? extends K, ? extends V> m) {
    this.loadFactor = DEFAULT_LOAD_FACTOR;
    putMapEntries(m, false);
}
```

### 插入元素：put，自动扩容

HashMap是添加元素前尽心扩容检查

HashMap的扩容时2倍扩容，扩容触发为size == threshold

空散列表的是直到插入第一个元素才扩容后到16

思路：先判断散列表是否为空>>在判断hash对应的槽是否为空 >> 再判断hash对应的槽的第一个元素key是否相同 >> 判断槽后的点是否为红黑树节点 >> 最后才遍历链表插入元素

遍历链表的思路：对遍历的次数进行累加（用于判断是否需要转换为红黑树）>> 未遍历完，取到key相同的节点 >> 遍历完（尾部插入，判断是否转换为红黑树）

奇怪的点：为什么不用while循环遍历链表，for循环遍历的思路不够while循环清晰

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

/**
 * Implements Map.put and related methods.
 *
 * @param hash hash for key
 * @param key the key
 * @param value the value to put
 * @param onlyIfAbsent if true, don't change existing value
 * @param evict if false, the table is in creation mode.
 * @return previous value, or null if none
 */
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    // tab指向哈希槽table，p表示当前遍历到的元素
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    // 当table为空即散列表为空时，先扩容到16
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    // 当hash对应的哈希槽为空时，直接赋值
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        // e表示桶中key相同的元素，k表示e的key
        Node<K,V> e; K k;
        // 先判断第一个元素，若hash相同，key相同或key不为null而值相等，则找到key相同的元素
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        // 当槽中第一个元素为红黑树节点时，将新元素插入树中，否则是在链表中插入
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        // 遍历链表
        else {
            for (int binCount = 0; ; ++binCount) {
                // 链表已经遍历完，没有keu相同的元素
                if ((e = p.next) == null) {
                    // 将新元素插入链表尾部
                    p.next = newNode(hash, key, value, null);
                    // 当桶中的链表长度等于8时，链表会转换为红黑树
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                // hash相同，key相同或key不为null而值相等，则e为元素，退出遍历
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                // 此时e指向p的下一个节点，将e赋给p即进行下一个节点的判断
                p = e;
            }
        }
        // 当e不为null时，返回旧元素的value
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    // 更新修改次数
    ++modCount;
    // 当散列表元素个数等于threshold时，进行2倍扩容
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    // 新元素被插入链表尾部，即返回null
    return null;
}
```

### 获取元素：get

整体思路和add()方法中遍历查找元素是类似的

```java
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}

/**
 * @param hash hash for key
 * @param key the key
 * @return the node, or null if none
 */
final Node<K,V> getNode(int hash, Object key) {
    // tab表示table数组，first表示hash对应的槽的第一个元素，e表示目标元素，k表示目标元素的key
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    // 若table不为null且不为空且first不为null则继续遍历查找
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & hash]) != null) {
        // 永远先判断第一个元素，减少遍历量，若未目标元素即返回
        if (first.hash == hash && // always check first node
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        if ((e = first.next) != null) {
            // 若first为红黑树节点，则到红黑树中查找并获取
            if (first instanceof TreeNode)
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            // 循环遍历查找
            do {
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
```

### HashMap中链表的结构

每个节点存储的信息：hash、key、value、next四个部分

```java
static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;
    V value;
    Node<K,V> next;

    Node(int hash, K key, V value, Node<K,V> next) {
        this.hash = hash;
        this.key = key;
        this.value = value;
        this.next = next;
    }

    public final K getKey()        { return key; }
    public final V getValue()      { return value; }
    public final String toString() { return key + "=" + value; }

    public final int hashCode() {...}
	// 返回旧元素value
    public final V setValue(V newValue) {...}

    public final boolean equals(Object o) {
        if (o == this)
            return true;
        if (o instanceof Map.Entry) {
            Map.Entry<?,?> e = (Map.Entry<?,?>)o;
            if (Objects.equals(key, e.getKey()) &&
                Objects.equals(value, e.getValue()))
                return true;
        }
        return false;
    }
}
```

### 视图：keySet()、values()、entrySet()

keySet、values、entrySet三种视图在HasMap中都有对应的Set内部类、Iterator内部类，用于迭代遍历等操作

```java
public Set<K> keySet() {
    Set<K> ks = keySet;
    if (ks == null) {
        ks = new KeySet();
        keySet = ks;
    }
    return ks;
}

// 对应的Set内部类
final class KeySet extends AbstractSet<K> {
    public final int size()                 { return size; }
    public final void clear()               { HashMap.this.clear(); }
    public final Iterator<K> iterator()     { return new KeyIterator(); }
    public final boolean contains(Object o) { return containsKey(o); }
    public final boolean remove(Object key) {
        return removeNode(hash(key), key, null, false, true) != null;
    }
    public final Spliterator<K> spliterator() {
        return new KeySpliterator<>(HashMap.this, 0, -1, 0, 0);
    }
    public final void forEach(Consumer<? super K> action) {
        Node<K,V>[] tab;
        if (action == null)
            throw new NullPointerException();
        if (size > 0 && (tab = table) != null) {
            int mc = modCount;
            for (int i = 0; i < tab.length; ++i) {
                for (Node<K,V> e = tab[i]; e != null; e = e.next)
                    action.accept(e.key);
            }
            if (modCount != mc)
                throw new ConcurrentModificationException();
        }
    }
}

// 对应的Iterator内部类
final class KeyIterator extends HashIterator implements Iterator<K> {
    public final K next() { return nextNode().key; }
}
```

```java
public Set<Map.Entry<K,V>> entrySet() {
    Set<Map.Entry<K,V>> es;
    return (es = entrySet) == null ? (entrySet = new EntrySet()) : es;
}

// 对应的Set内部类
final class EntrySet extends AbstractSet<Map.Entry<K,V>> {
    public final int size()                 { return size; }
    public final void clear()               { HashMap.this.clear(); }
    public final Iterator<Map.Entry<K,V>> iterator() {
        return new EntryIterator();
    }
    public final boolean contains(Object o) {
        if (!(o instanceof Map.Entry))
            return false;
        Map.Entry<?,?> e = (Map.Entry<?,?>) o;
        Object key = e.getKey();
        Node<K,V> candidate = getNode(hash(key), key);
        return candidate != null && candidate.equals(e);
    }
    public final boolean remove(Object o) {...}
    public final Spliterator<Map.Entry<K,V>> spliterator() {...}
    public final void forEach(Consumer<? super Map.Entry<K,V>> action) {...}
}

// 对应的Iterator内部类
final class EntryIterator extends HashIterator
    implements Iterator<Map.Entry<K,V>> {
    public final Map.Entry<K,V> next() { return nextNode(); }
}
```