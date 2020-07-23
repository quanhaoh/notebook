## 常用方法

### 获取功能的方法

- `String getAbsolutePath()`

返回File的绝对路径名字符串

- `String getPath()`

将File转换为路径名字符串，即构造方法中传递的字符串，toString方法也是调用getPath方法

- `String getName()`

返回此File对象表示的文件或目录的名称，即获取构造方法传递路径的结尾部分（文件夹/文件名）

- `long length()`

返回此File对象表示的文件长度，以字节为单位

若构造的路径不存在，则返回0

### 判断功能方法

- `bolean exits()`

判断此File对象表示的文件或目录是否实际存在

- `boolean isDirectory()`

判断此File对象是否为目录

- `boolean isFile()`

判断此File对象是否为文件

### 创建、删除功能方法

- `boolean createNewFile()`

当File对象指向的文件不存在，则创建文件，返回true；否则不创建，返回false

File指向的路径必须存在，否则会抛出异常

- `boolean mkdir()`

创建**单级**文件夹

当File对象指向的文件夹不存在，则创建文件夹，返回true；否则不创建，返回false

若File对象指向的是文件，也会创建对应名称的文件夹，而不是文件

- `boolean mkdirs()`

创建**多级**文件夹

当File对象指向的文件夹不存在，则创建文件夹，包括任何必须但不存在父目录，返回true；否则不创建，返回false

若File对象指向的是文件，也会创建对应名称的文件夹，而不是文件

- `boolean delete()`

**彻底删除**此File表示的文件或目录，返回true

### 遍历目录功能方法

- `String[] list()`

返回String对象表示目录中的所有文件和文件夹（包括隐藏文件夹/文件）

该目录必须实际存在，否则会抛出异常

- `File[] listFiles()`

返回File对象表示目录中的所有文件和文件夹（包括隐藏文件夹/文件），并且封装为File对象

该目录必须实际存在，否则会抛出异常