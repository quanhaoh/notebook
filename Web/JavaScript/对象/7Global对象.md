## Global对象

全局对象

### 1. 创建方式

Global中封装的方法不需要对象就可直接调用

- 方法名();

### 2. 方法

#### encodeURI(参数)方法

url编码，将传入的参数进行编码

#### decodeURI(enCodeURI()变量)方法

url解码，将传入的参数进行解码

#### encodeURIComponent(参数)方法

url编码，将传入的参数进行编码，相对于encodeURI编码会更长

#### decodeURIComponent(enCodeURI()变量)方法

url解码，将传入的参数进行解码

```javascript
var name = "hong";
var encode = encodeURI(name);
var nameDecode = decodeURI(encode);

var name = "hong";
var encode = encodeURIComponent(name);
var nameDecode = decodeURIComponent(encode);
```

#### parseInt(参数)

将字符串转为数字：逐一判断字串的字符是否为数字，直到遇到非数字的字符，即将前面的字符转换为数字

```javascript
var str = "123c321
var number = paseInt(str);	// 结果为123
```

#### isNaN(参数)

判断变量是否为NaN

```javascript
var s = NaN;
var p = isNaN(s)	// 结果为true
```

#### eval(参数)

将字符串转换为脚本代码来执行

```javascript
var str = "alter(123)";
eval(str);
```

