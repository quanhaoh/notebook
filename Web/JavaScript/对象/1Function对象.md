## Function对象

### 1. 创建方式

#### 方式1：类似于Java创建对象的方式

- var 方法名= new function(参数列表, 方法体);

#### 方式2：类似于Java创建函数的方式

- function 函数名(参数1， 参数2...){方法体};

```javascript
function showName(a, b){
	alert(a);
}

showName("a")；
```

#### 方式3：类似于Java的匿名内部类

- var 方法名 = 函数名(参数1， 参数2...){方法体};

```javascript
var showName = function(a, b){
	alert(a);
}

showName(a);
```

### 2. 属性

- length：形参个数

### 3. 特点

1. 定义方法时，形参的类型不需要写
2. 方法是一个对象，若定义同名方法，则会覆盖
3. 方法的调用与参数列表无关只与函数名有关，即不存在重载的理解，也可以传入任意的参数
4. 再方法中声明有一个隐藏的内置对象（数组）：arguments，用于存储所有传入的参数

### 4. 调用

- 方法名(参数列表);

 