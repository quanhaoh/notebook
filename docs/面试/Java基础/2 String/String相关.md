## String相关

1. String是如何实现的

   byte数组、char数组

   Java9后引进了byte数组的形式，因为拉丁字母不需要占用2个字节的char，以此节约资源

2. 常用方法

   equals(Object object)

   - 直接使用==判断
   - 判断是否是String对象（instanaceof关键词），是的话转换为char数组遍历比较

   equalsIgnoreCase(String string)

   - 忽略大小写字母
   - 比较两个字符串的大小，同样是转换为字符数组，遍历比较，有不同直接返回char1-char2，返回ASCII码的差值

   length()

   - 

   indexOf()

   - 

   lastIndexOf()

   - 

   contaians()

   - 

   toLowerCase()

   - 

   toUpperCase()

3. 为什么String要用final修饰？

   安全：防止在使用过程中被修改，违背了String创建的初衷

   高效：比如常量池的实现也要依赖于final，如果不是final，就没办法实现常量池，因为会被修改

4. ==和equals()有什么区别？

   ==对于基本数据类型直接比较值，对于引用对象是比较其引用地址

   equals比较的是字符串是否相等，而Object类的equals()比较的是引用地址，String重写了该方法

5. String、StringBuilder和 StringBudder有什么区别？

   String具有非常强大的功能，但是在字符串的修改方面比如拼接，需要耗费大量的资源，每	次修改都要重新创建String对象

   StringBuffer是对String的改进，在拼接字符串时不会新建对象，节约资源，而且是线程安	全的，使用synchronized修饰方法实现

   StringBuilder是对StringBuffer的补充，也具有节约资源的效果，但不是线程安全的，在非	高并发的场景使用StringBuilder拼接会更适合

6. String的intern()方法有什么含义？String类型在JVM中如何存储，编译器对String做了哪些优化？

   使用intern可以提示JVM将字符串缓存起来，提高效率。在Java8之前是放在JVM内存的永久	代中，这样会有可能占满导致OOM错误；而在Java8之后永久代变成了元空间，而且缓	存空间也做了适当的扩充

   在编译器层面，会自动进行排重，将多个String对象指向同一个字符串

   