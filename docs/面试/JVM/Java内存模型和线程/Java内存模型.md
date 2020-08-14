## Java内存模型

### 硬件的效率问题及硬件内存架构

CPU的处理速度与I/O操作的速度相差几个量级，因此在二者间加入高速缓存

共享内存多核系统：每个处理器都有自己的高速缓存，但共享一个主内存

<img src="https://gitee.com/quanhaoh/blogImage/raw/master/img/硬件内存架构图1.png" alt="硬件内存架构" style="zoom:80%;" />

<img src="https://gitee.com/quanhaoh/blogImage/raw/master/img/硬件内存架构图2.png" alt="硬件内存架构图2" style="zoom: 67%;" />

### Java内存模型

Java内存模型中的主内存、工作内存都是虚拟机内存中的一部分，与硬件的主内存没有关系，但可以类比

Java内存模型与Java内存区域划分没有关系，但可以简单类比：工作内存类比为栈、主内存类比为堆和方法区

<img src="https://gitee.com/quanhaoh/blogImage/raw/master/img/Java内存模型.jpg" alt="Java内存模型" style="zoom:60%;" />

#### 内存间的交互操作：8种原子操作

| 操作   | 作用区域             | 说明                                                |
| ------ | -------------------- | --------------------------------------------------- |
| lock   | 主内存共享对象       | 加锁，线程独占共享变量，阻塞同步                    |
| unlock | 主内存共享对象       | 释放锁                                              |
| assign | 工作内存共享对象副本 | 赋值，将从执行引擎获取的值赋给共享对象副本          |
| use    | 工作内存共享对象副本 | 使用，将共享副本的值传递给执行引擎                  |
| load   | 工作内存共享对象副本 | 加载，把read操作从主内存得到的共享变量值赋给副本    |
| store  | 工作内存共享对象副本 | 存储，把副本的值传到主内存中                        |
| read   | 主内存共享对象       | 读取，从主内存中读取共享变量值并传到工作内存中      |
| write  | 主内存共享对象       | 写入，把store操作从工作内存得到的副本之赋给共享变量 |

JVM对于**基本数据类型**变量的assign/use、load/store、read/write操作是原子性的

若要进行更大范围的原子性操作，即需要用到lock/unlock操作，代码中使用即synchronized同步

#### long和double类型的非原子性协定

对于64位的long和double，虚拟机允许没有被volatile修饰的变量划分为两次32位的操作来进行，因此load/strore、read/write便不是原子性的

对于32位的Hotspot虚拟机，long类型变量有非原子性操作的风险

- 提供了-XX:AlwaysAtomicAccess参数来约束虚拟机对、所有类型进行原子性操作

对于double类型变量，无论是64位还是32位虚拟机，通常都不会产生非原子性操作的风险不能你