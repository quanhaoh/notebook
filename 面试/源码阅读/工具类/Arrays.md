## Arrays

对数组进行一些高效操作：查找、拷贝、排序、相等判断、批量填充

Arrays工具类使用的数据类型包括：8种基本数据类型、Object、泛型T等，可以说凡是数组都支持

Arrays中的每个方法几乎都有两个版本：数组全局范围操作、数组局部范围操作

- 全局范围操作的源码几乎都是调用局部范围操作，范围为：0 ~ length - 1

### binarySearch()

==所查找的数据必须是有序的，否则二分查找无法使用==

包括全局范围查询、局部范围查询

- binarySearch0(long[] a, long key)
- binarySearch0(long[] a, int fromIndex, int toIndex, long key)
- binarySearch(数组, key， int fromIndex, int toIndex)

```java
// 无起始、结束范围也是调用以下方法实现，不用递归实现的原因是：容易发生堆栈溢出
private static int binarySearch0(long[] a, int fromIndex, int toIndex, long key) {
    int low = fromIndex;
    int high = toIndex - 1;
	// 循环结束条件
    while (low <= high) {
        // 求出中点
        int mid = (low + high) >>> 1;
        long midVal = a[mid];

        if (midVal < key)
            low = mid + 1;
        else if (midVal > key)
            high = mid - 1;
        else
            return mid; // key found
    }
    return -(low + 1);  // key not found.
}
```

### copyOf

复制数组，本质上是调用System类的arraycopy方法

```java
// 直接调用Sysy的本地方法arraycopy
public static byte[] copyOf(byte[] original, int newLength) {
    byte[] copy = new byte[newLength];
    System.arraycopy(original, 0, copy, 0, Math.min(original.length, newLength));
    return copy;
}

// 带范围的复制
public static byte[] copyOfRange(byte[] original, int from, int to) {
    int newLength = to - from;
    if (newLength < 0)
        throw new IllegalArgumentException(from + " > " + to);
    byte[] copy = new byte[newLength];
    System.arraycopy(original, from, copy, 0,
                     Math.min(original.length - from, newLength));
    return copy;
}

// 将任意类型的原数组，复制到任意类型的目标数组中
public static <T,U> T[] copyOf(U[] original, int newLength, Class<? extends T[]> newType) {}


// System类的arraycopy方法
@HotSpotIntrinsicCandidate
public static native void arraycopy(Object src,  int  srcPos,
                                    Object dest, int destPos,
                                    int length);
```

### sort()：快速排序

```java
static void sort(int[] a, int left, int right,
                 int[] work, int workBase, int workLen) {
    // Use Quicksort on small arrays
    if (right - left < QUICKSORT_THRESHOLD) {
        sort(a, left, right, true);
        return;
    }

    /*
     * Index run[i] is the start of i-th run
     * (ascending or descending sequence).
     */
    int[] run = new int[MAX_RUN_COUNT + 1];
    int count = 0; run[0] = left;

    // Check if the array is nearly sorted
    for (int k = left; k < right; run[count] = k) {
        // Equal items in the beginning of the sequence
        while (k < right && a[k] == a[k + 1])
            k++;
        if (k == right) break;  // Sequence finishes with equal items
        if (a[k] < a[k + 1]) { // ascending
            while (++k <= right && a[k - 1] <= a[k]);
        } else if (a[k] > a[k + 1]) { // descending
            while (++k <= right && a[k - 1] >= a[k]);
            // Transform into an ascending sequence
            for (int lo = run[count] - 1, hi = k; ++lo < --hi; ) {
                int t = a[lo]; a[lo] = a[hi]; a[hi] = t;
            }
        }

        // Merge a transformed descending sequence followed by an
        // ascending sequence
        if (run[count] > left && a[run[count]] >= a[run[count] - 1]) {
            count--;
        }

        /*
         * The array is not highly structured,
         * use Quicksort instead of merge sort.
         */
        if (++count == MAX_RUN_COUNT) {
            sort(a, left, right, true);
            return;
        }
    }

    // These invariants should hold true:
    //    run[0] = 0
    //    run[<last>] = right + 1; (terminator)

    if (count == 0) {
        // A single equal run
        return;
    } else if (count == 1 && run[count] > right) {
        // Either a single ascending or a transformed descending run.
        // Always check that a final run is a proper terminator, otherwise
        // we have an unterminated trailing run, to handle downstream.
        return;
    }
    right++;
    if (run[count] < right) {
        // Corner case: the final run is not a terminator. This may happen
        // if a final run is an equals run, or there is a single-element run
        // at the end. Fix up by adding a proper terminator at the end.
        // Note that we terminate with (right + 1), incremented earlier.
        run[++count] = right;
    }

    // Determine alternation base for merge
    byte odd = 0;
    for (int n = 1; (n <<= 1) < count; odd ^= 1);

    // Use or create temporary array b for merging
    int[] b;                 // temp array; alternates with a
    int ao, bo;              // array offsets from 'left'
    int blen = right - left; // space needed for b
    if (work == null || workLen < blen || workBase + blen > work.length) {
        work = new int[blen];
        workBase = 0;
    }
    if (odd == 0) {
        System.arraycopy(a, left, work, workBase, blen);
        b = a;
        bo = 0;
        a = work;
        ao = workBase - left;
    } else {
        b = work;
        ao = 0;
        bo = workBase - left;
    }

    // Merging
    for (int last; count > 1; count = last) {
        for (int k = (last = 0) + 2; k <= count; k += 2) {
            int hi = run[k], mi = run[k - 1];
            for (int i = run[k - 2], p = i, q = mi; i < hi; ++i) {
                if (q >= hi || p < mi && a[p + ao] <= a[q + ao]) {
                    b[i + bo] = a[p++ + ao];
                } else {
                    b[i + bo] = a[q++ + ao];
                }
            }
            run[++last] = hi;
        }
        if ((count & 1) != 0) {
            for (int i = right, lo = run[count - 1]; --i >= lo;
                b[i + bo] = a[i + ao]
            );
            run[++last] = right;
        }
        int[] t = a; a = b; b = t;
        int o = ao; ao = bo; bo = o;
    }
}
```

### fill()

```java
public static void fill(long[] a, long val) {
    for (int i = 0, len = a.length; i < len; i++)
        a[i] = val;
}
```

### asList(T..a)

Arrays中定义有内部类：ArrayList  implements AbstractList

该内部类包含Collection的方法，其对象是==定长列表==

- 包含操作ArrayList元素的各种方法，但不能扩容（列表长度固定），可以改查

- 该内部类中定义了一个final数组（不可变），对列表的操作即对该数组的操作

```java
@SafeVarargs
@SuppressWarnings("varargs")
public static <T> List<T> asList(T... a) {
    return new ArrayList<>(a);
}
```

```java
// 获取列表长度
public int size() {}

// 将列表转为对应的数组
public Object[] toArray() {}


public <T> T[] toArray(T[] a) {}

// 获取指定的元素
@Override
public E get(int index) {}

// 修改指定元素的值
@Override
public E set(int index, E element) {}

// 获取指定元素的坐标
@Override
public int indexOf(Object o) {}

// 判断是否含有该元素
@Override
public boolean contains(Object o) {}

@Override
public Spliterator<E> spliterator() {}

// 遍历列表，执行对应action
@Override
public void forEach(Consumer<? super E> action) {}

@Override
public void replaceAll(UnaryOperator<E> operator) {
}

// 快速排序
@Override
public void sort(Comparator<? super E> c) {}

// 获取迭代器，Arrays内部定义了一个迭代器内部类，用于迭代遍历列表
@Override
public Iterator<E> iterator() {}
```

### toString()

返回数组的字符串形式

### equals()

比较两个数组是否相等

### deepEquals()

比较两个数组是否深度相等