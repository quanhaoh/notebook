## HashSet

### 重要成员变量

```java
static final long serialVersionUID = -5024744406713321676L;

// HashSet的方法实现是基于HashMap实现的
private transient HashMap<E,Object> map;

// 用于填充HashMap的value，没有实际意义
private static final Object PRESENT = new Object();
```

```java
public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}

public boolean remove(Object o) {
    return map.remove(o)==PRESENT;
}

public Iterator<E> iterator() {
    return map.keySet().iterator();
}
```