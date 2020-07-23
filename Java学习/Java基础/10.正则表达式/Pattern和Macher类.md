## Pattern和Matcher类

```java
Pattern pattern = Pattern.compile("(.*)\\(.*(\\.pdf)");
Matcher matcher = pattern.matcher(file.getName());
if(matcher.find()) {
    String name = matcher.group(1) +matcher.group(2);
}
```

### Pattern类

创建Pattern对象

- Pattern构造方法为私有
- 通过工厂方法：compile获取对象

### Matcher类

创建Matcher对象

- Matcher构造方法私有
- 通过pattern对象的matcher方法获取

#### 常用方法

Matcher.matches(String pattern, String target)

- 判断target是否与表达式匹配

matcher(Pattern pattern)



索引方法

查找方法

- find()

替换方法

- replaceFirst()
- replaceAll()