## 思想  
通过URL的特性，下载类似的URL，如URL的路径名仅服务器ID不同，即可批量下载。

## 所需知识


### 1、URL中常包含页面别名（如国家/地区名）
在抓取过程中，页面别名常可省略，只保留数据库ID，仍可正常加载


### 2、迭代器工具itertools
生成迭代器，用于循环
- itertools.count(n,step)生成无穷的迭代器，步进长度为step，初始为n 

### 3、字符串格式化函数format('attibution1', 'attibution2',...)
eg: "{0} {1}".format("hello", "world")  
> 设置指定位置，也可默认顺序，用于连接字符串  
- 可有无穷参数
- 可设置指定参数
- 可用于格式化数字，如保留小数位数等