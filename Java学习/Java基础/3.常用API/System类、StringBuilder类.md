### System类
#### 1、currentTimeMills()方法
返回以毫秒为单位的当前时间，可用于测试程序的效率（程序执行前后分别获取）
#### 2、arraycopy()方法
将原数组的指定数据拷贝到另一个数组中

### 二、StringBuilder类
即字符串缓冲区，可以提高字符串的操作效率，底层也是一个数组，但数组长度可以改变

大量进行字符串拼接时，不可用 + 或者 concat() 方法
- 这两种方法每次拼接都会新建一个 String 对象、分配空间，会极大降低效率

#### 1、StringBuffer类

```
StringBuffer str1 = new StringBuffer(); // 构造默认缓存空间为 16 位的对象
StringBuffer str1 = new StringBuffer(int capaciy);   // 构造指定缓存容量的对象
StringBuffer str1 = new StringBuffer(String str);   //构造指定字符串值的对象

str1.append(str2); //拼接 str2 
str1.toString();    //转换为 String 类型
```


常用方法 |说明
---|---
append(str) | 在末尾拼接 str
reverse() |颠倒字符串顺序
delete(startIndex, endIndex) |删除从 startIndex 到 endIndex-1 的字符
replace(start, end, str) |将 start 到 end-1 的字符替换为 str




#### 2、StringBuilder类
与 StringBuffer 的区别是，StringBuilder 不是线程安全的，不能同步访问,是异步的

用法与 StringBuffer 类似

### 基本类型包装类