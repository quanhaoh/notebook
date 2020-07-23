## JSON数据和Java对象的转换

### JSON解析器

封装好的工具类

### JSON数据转换为Java对象

使用步骤：

1. 倒入jackjson的jar包
2. 创建JackJson核心对象：ObjectMapper
   - ObjectMapper mapper = new ObjectMapper();
3. 调用ObjectMapper对象的方法进行转换，将JSON数据转换为特定对象
   - Obj obj = readValue(json字符串数据, obj.Class)

### Java对象转换为JSON

将类对象的成员变量及对应的值转换为JSON数据格式

使用步骤：

1. 倒入jackjson的jar包
2. 创建JackJson核心对象：ObjectMapper
   - ObjectMapper mapper = new ObjectMapper();
3. 调用ObjectMapper对象的方法进行转换
   - mapper.writeValue(参数1， obj)
     - File对象，将obj转换为JSON并保存到指定文件
     - Writer对象 ：将obj转换为JSON并填充到字符输出流中
     - OutputStream：将obj转换为JSON并填充到字节输出流中
   - String json = mapper.writerValueAsString(obj)：将对象obj转换为JSON字符串
4. 设置注解：在类定义的对象前使用注解，使用方法和@Override相同
   - @JsonIgnore：排除属性，当进行JSON转换时，会自动忽略该对象
   - @JsonFormat()：格式化对象，但进行JSON转换时，按照注解的格式进行转换，比如Date()对象的格式化
5. 复杂Java对象的转换：如List、Map

- List对象会自动转换成[{}, {}, {}]数组的格式，每个{}都是List集合中的一个对象
- Map对象会将key和value与JSON的一一对应进行转换，和普通对象转换的格式相同