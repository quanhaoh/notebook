## Properties集合

Propertie类表示一个持久的属性集，是唯一和IO流相结合的集合，继承了HashTable集合， key和value默认是字符串

==常用于配置文件的读写==

### 数据的持久化

将数据持久化：store()

- store(OutputStream out, String comment)
  - 默认是utf8编码，不能有中文
  - comment为注释，不能是中文
- store(Writer writer, String comment)
  - 可以有中文

读取持久化的数据

- load(InputStream in)
  - 默认是utf8编码，键值对不能有中文
- load(Reader reader)
  - 可以有中文

### 常用方法

添加数据

- Object setProperty(String key, String value)

获取数据

- String getProperty(String key)

获取所有的键集合

- Set<String> stringPropertyNames()

