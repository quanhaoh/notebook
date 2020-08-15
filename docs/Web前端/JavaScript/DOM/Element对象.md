## Element对象

### 1. 获取方法

通过document对象获取

### 2. 方法

#### setAttribute(属性名, 属性值)：添加属性

#### removeAttribute(属性名)：删除属性

### 3.操作Element对象：

1. 修改属性值：

   - 获取元素对象

   - 查看API文档：确定哪些属性可以修改

2. 修改标签体内容

   - 获取元素对象
   - 使用innerHTML属性修改标签体内容  

```html
<p id="p1">修改前的文字</p>

<script>
    alert("修改")
	var p1 = document.getElementById("P1");
	p1.innerHTML = "修改后的内容";
</script>
```

