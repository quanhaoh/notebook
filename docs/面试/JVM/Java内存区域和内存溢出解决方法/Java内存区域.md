## Java内存区域

### 线程共享

##### 堆

- 存放对象数据

#### 方法区

- 存放类加载数据、常量、静态变量、即时编译器编译后的代码等

### 线程私有

##### 程序计数器

- 字节码解释器改变计数器的值来依次读取指令，实现代码的流程控制：如分支、循环、跳转、异常处理、线程恢复等
- 多线程状态下，程序计数器会记录线程切换时当前线程执行的位置，以便恢复线程

##### 虚拟机栈

- 每个Java方法都会建立一个虚拟机栈，用于存储局部变量表、对象引用、操作数、常量池引用等

##### 本地方法栈

- 作用与虚拟机栈类似，为虚拟机的native方法服务

##### 为什么需要私有

程序计数器私有主要是为了可以进行线程切换后可以恢复之前的执行位置

虚拟机栈和本地方法栈私有主要是为了方法的局部变量不被别的线程访问