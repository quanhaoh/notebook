### 常用方法

-  `addAll(Collection<T> e,  T.. elements)`

批量向集合中添加同类元素

- `shuffle(List<?> list)`

打乱集合中的元素顺序

- `sort(List<T> list)`

将集合中的元素按默认规则排序，即升序

要对自定义对象排序，该类要实验Comparable接口中的comparaTo()方法

自定义的comparaTo（）规则：this.参数 - 传参（升序）/ 传参-this.参数（降序）

- `sort(List<T> list, Comparator<? super T>)`

将集合中的元素按指定规则排序

Comparator集合与Compable集合类似，其定义了一个compare方法，在使用的时候需重写其比较规则