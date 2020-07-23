## Jsoup解析器

### 1. 解析步骤

1. 导入Jsoup.jar包
2. 获取xml文件的path

```java
String path = Demo.class.getClassLoader().getResource(xml文件名).getPath();
```

3. 解析xml文档，加载文档进内存，获取DOM树即Document对象

```java
Document document = Jsoup.parse(new File(path), "utf-8");
```

4. 获取元素对象Element

```java
// 返回一个Element对象的ArrayList集合
Elements elements = document.getElementsByTag("标签名");
```





