## File类

### 概述

File类是文件和目录路径名的抽象表示，主要用于文件和目录的创建、查找、删除等操作

File类的方法

- 创建一个文件/文件夹
- 删除文件/文件夹
- 获取文件/文件夹
- 判断文件/文件夹是否存在
- 遍历文件夹
- 获取文件大小

 ### File类的静态成员变量

`String pathSeparator`：与系统有关的路径分隔符，windows中为`；`，Linux中为`：`

`char pathSeparatorChar`：与系统有关的路径分隔符

>  两者输出结果是一样的，只是数据类型不同



`String separator`：与系统有关的默认名称分隔符，windows中为`\`，Linux中为`/`

`char separatorChar`：与系统有关的默认名称分隔符

>  两者输出结果是一样的，只是数据类型不同

### 绝对路径和相对路径

路径不区分大小写

绝对路径：以盘符开始的路径(windows)、以/开始的路径(Linux)

相对路径：相对于当前项目的根目录

### 构造方法

`File(String pathname)`

pathname为路径名称，无论是否存在都可以，绝对路径和相对路径都可以

`File(String parent, String child)`

将路径分为父路径和子路径，父子路径可以单独定义，变化灵活

`File(File parent, String child)`

将路径分为父路径和子路径，父子路径可以单独定义，变化灵活

父路径是File类型，可以用File类的方法处理路径，再使用路径来创建 对象