## Jsoup对象

### 1. Jsoup

工具类，用于解析html或sml文档，返回Document

- parse(File in, String charsetName)：解析xml或html文件
- parse(String html)：解析xml或html字符串
- parse(URL url, int timeoutMillers)：通过网络路径获取指定的html或sml的文档对象

### 2. Document

用于获取Element对象

- getElementsByTag(String tagName)：根据标签名获取对象
- getElementsById(String id)：根据id属性值获取对象
- getElementsByAttribute(String attribute)：根据属性名获取对象
- getElementsByAttributeValue(String attribute, String value)：根据属性名和值获取对象

### 3. Element

#### 获取属性值

- String attr(String key)：根据属性名获取其属性值

#### 获取文本内容

- text()：获取所有子标签的文本内容
- html：获取标签体的所有内容，包括标签名

### 4. Node

