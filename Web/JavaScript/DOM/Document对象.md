## Dcument对象

### 1. 获取方法

- window.document
- document

### 2. 方法

#### 获取Element对象：

1. getElementById("id")：根据id属性获取元素，id要唯一
2. getElementByTagName("id")：根据元素名称获取同名元素对象数组
3. getElementByClass("class")：根据class属性获取同名元素对象数组
4. getElementBName("name")：根据name属性获取同名元素对象数组

#### 创建DOM对象：

1. createAttribute("元素标签名称")：返回一个对应标签的Element对象
2. createComment()
3. createElement()
4. createTextNode()

### 3. 属性

#### 可获取URL中各个部分

1. href：完整的url
2. path：路径
3. host：主机
4. hostname：主机名
5. port：端口

#### 获取DOM对象

1. windoe.document.getElementById("id")
   - 不常用，一般是直接获取

