## 查看函数


### 1、compile(pattern, flags=0)

将正则表达式创建为一个正则表达式对象，便于使用match()、search()等方法
- 当正则表达式需要多次使用时，较为方便
- flags参数可为re变量，如IGNORECASE等，添加匹配条件
```
prog = re.compile(pattern)
result = prog.match(string)
```
### 2、search(pattern, string, flags=0)
在string的任意位置进行查找、匹配

### 3、match(pattern, string, flags=0)
从string的开始进行匹配，若不匹配则返回None

### 4、fullmatch(pattern, string, flags=0)
若string完全匹配pattern的模式，则返回string

### 5、findall(pattern, string, flags=0)
寻找所有匹配的字符串，并返回一个元组

### 6、finditer(pattern, string, flags=0)
寻找所有匹配的字符串，并返回一个迭代器，相当于


## 处理字符串

### 1、split(pattern, string, maxsplit=0, flags=0)
根据正则表达式规则，对string进行分解，并返回分解后的结果

```
re.split('[a-f]+', '0a3B9', flags=re.IGNORECASE)
['0', '3', '9']
```

### 2、sub(pattern, repl, string, count=0, flags=0)


## 其他

### match.group([group1, ...])  
group(0)返回匹配的整个结果  
group(1)返回第一个group的结果  
group(n)返回第n个group的结果  
group()返回所哟group，以元组的形式
