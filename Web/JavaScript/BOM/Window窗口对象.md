## Window窗口对象

### 1. 使用方式

Window对象封装的方法不需要创建对象即可直接使用

- 方法名();

### 2. 方法

#### 弹出窗口的方法

1. alert(字符串);
   - 弹出警告窗口，显示字符串
2. confirm(字符串);
   - 弹出确认按钮窗口，输入的参数作为提示信息
   - 确认则返回true，取消则返回false
3. prompt(字符串);
   - 弹出可输入的对话框，输入的参数作为提示信息
   - 在对话框输入的文字作为函数返回值

```html
<button id="p1">提交</button>

<script>
    var p1 = document.getElementById("p1");
    p1.onclick = function () {
        alert("提交后不可更改");
        confirm("确认提交吗");
        var text = prompt("输入信息后提交");
        alert(text);
    };
</script>
```

#### 打开关闭方法

1. open(url)
   - 打开新窗口,url指定新窗口i的网址，返回一个窗口对象
2. close()
   - 关闭浏览器窗口，哪个窗口对象调用即关闭其对应的窗口

```html
<button id="p1">打开百度</button>
<button id="p2">关闭新打开的百度窗口</button>
<button id="p3">关闭当前窗口</button>

<script>
    var p1 = document.getElementById("p1");
    var p2 = document.getElementById("p2");
    var p3 = document.getElementById("p3");
    var newWindow;
    p1.onclick = function () {
         newWindow= open("http://baidu.com");
    };
    p2.onclick = function () {
        newWindow.close();
    };
    p3.onclick = function () {
        close();
    };
</script>
```

#### 定时器

1. setTimeout(代码, 毫秒值)

   - 一次性定时器

   - 代码参数既可以是代码也可以是函数，毫秒值即指定延迟的时间

   - 在指定的毫秒数后执行函数或代码
   - 返回唯一标识符，作为clearTimeout函数的参数来取消定时器

2. clearTimeout(标识符)

   - 取消一次性定时器

3. setInterval(代码, 毫秒)

   - 循环定时器，使用方法与setTimeout相同
   - 返回唯一标识符，作为clearInteval函数的参数来取消定时器

4. clearInteval(标识符)

   - 取消循环定时器

### 3. 属性

#### 获取其他BOM对象

1. history
2. location
3. navigator
4. Screen

#### 获取DOM对象

1. windoe.document.getElementById("id")
   - 不常用，一般是直接获取

