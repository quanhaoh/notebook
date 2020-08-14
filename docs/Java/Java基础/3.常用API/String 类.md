### 一、String 对象的3种构造方式

**String 效果上相当于char[]字符数组，但本质上是byte[]字节数组**


```
String str1 = "hong";
String str2 = new String(); // 默认构造器
String str3 = new String(str1);   // 带参构造器

//*字符数组转化为字符串*/
char[] chArray = {'a', 'b', 'c'}
String str4 = new String(chArray);

//*字节数组转化为字符串*/
byte[] byteArray = {104,101,108,108,111};
String str5 = new String(byteArray);

//*字节数组和指定字符集转化为字符串*/
byte[] byteArray = {104,101,108,108,111};
String str5 = new String(byteArray, “UTF-8");

```

### 二、字符串常用方法

方法 | 说明
---|---
length() | 返回字符串长度
charAt(i) | 返回字符串中第 i+1 个字符
concat(str) | 拼接str 字符串
toUpperCase() | 将字符串中的字母换为大写
toLowerCase() | 将字符串中的字母换为小写
char[] toCharArray() | 将字符串换为字符数组
equals(str) | 对两个字符串的值进行比较，返回 true/false
equalsIgnoreCase(str) | 忽略大小写，对两个字符串的值进行比较
startWith(str) | 判断字符串是否以某前缀开始，返回 true/false
startWith(str) | 判断字符串是否以某后缀结束，返回 true/false
contains(str) | 判断字符串是否包含某子串，返回 true/false
indexOf(str) | 返回字符串中出现的第一个 str 的下标，没有则返回 -1
indexOf(str，index) | 返回字符串中 index 后出现的第一个 str 的下标，没有则返回 -1
lastIndex(str) | 返回字符串中出现的最后一个 str 的下标，没有则返回 -1
lastIndex(str) | 返回字符串中 index 后出现的最后一个 str 的下标，没有则返回 -1
substring(startIdex) | 返回startIdex 到结束的子串
substring(startIdex，endIdex) | 返回startIndex 到 endIdex-1 的子串



### 三、字符串常量池
字符串常量池位于堆中

使用双引号直接创建的String对象，都位于常量池中，new创建的String不在常量池中

直接创建的字符串可称为字符串常量

```
String str1 = "hong";
String str2 = new String("hong");
boolean isEqual = (str1 == str2); //结果为 false，因为==比较的是对象地址
boolean isEqual = str1.equals(str2);  // 结果为true，equals方法比较的是对象内容
```