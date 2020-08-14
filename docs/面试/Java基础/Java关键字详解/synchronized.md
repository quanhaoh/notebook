## synchronized

对象锁和类锁之间互不干扰，二者是独立的，一个线程可以同时拥有两把锁也可以分别拥有一把锁

#### 1、synchronized可以作用在哪里？

##### 对象锁：修饰方法、修饰代码块并指定非静态变量（对象）

每个类的实例都有自己的一把锁，因为JVM中每个实例都要自己的引用和堆内存，不同实例之间的锁互不干扰

但若多个线程同时使用同一个实例时，另一个线程必须等待对象锁的释放才可以使用

对象锁不影响非同步方法的使用

- 方法形式：锁定对象为this
- 同步代码块，默认锁定对象是this，也可以是特定对象

```java
public class InstanceClass {
    public void main() {
        Stting str = new String
        // 锁对象为InstanceClass的this，当前实例
        synchronized(this) {
            ...
        }	
        // 锁对象为str字符串对象
        synchronized(str) {
            ...
        }	
	}
    
    // 锁对象为this，当前实例
    public synchronized void method() {
        ...
    }
}

```

##### 类锁：修饰静态方法、指定锁对象静态变量/Class对象

所谓类锁，也就是类对象锁，多线程使用类对象时需要获取锁，调用静态变量/静态方法/静态代码块或指定锁对象为Class对象的代码块，都需要用到类对象即Class对象，这些都在JVM中的堆或方法区中，所有线程共用

```java
public class InstanceClass {
    public static var;
    public void method1() {
        // 锁对象为静态对象
        synchronized(var) {
            ...
        }	
    }
    
    // 锁住类对象
    public static synchronized void method2() {
        ...
    }
    
    public void method3() {
        // 锁对象Class对象
        synchronized(InstanceClass.Class) {
            ...
        }	
	}
}
```

##### 常用加锁方式

```java
// 锁住this
synchronized(this) {
    ...
}	

// 只锁住实例中的一个方法，使用额外的变量，高并发下常用的锁住方法的做法
Object lock = new Object();
public void method1() {
    // 锁对象为静态对象
    synchronized(lock) {
        ...
    }	
}	

// 锁住Class对象
synchronized(InstanceClass.Class) {
    ...
}	
```

### 2、synchronized如何保证保证线程安全？即如何实现加锁和释放锁、如何实现可重入、如何保证可见性？

#### 加锁和释放锁的原理

synchronized修饰的块结构在编译成字节码文件后，块结构前后有monitorenter、monitorexit两个指令

- monitorenter：获得锁，锁计数器变为1

- monitorexit：该对象的锁计数器减1

锁计数器为0：锁空闲，线程可以获取锁

锁计数器不为0：锁已被线程获取，其他线程只能加入同步队列挂起（进入BLOCKED状态），等待获取锁

每个对象只与一个monitor（锁）关联，monitor只能被一个线程拥有，线程用完锁后必须释放锁即锁计数器减1

所谓加锁、重入锁，即使锁计数器加1；释放锁即锁计数器为0

#### 实现可重入的原理

线程通过锁计数器实现可重入而不需要重复获得锁，即不会重复执行monitorenter指令

当线程调用锁的其他块结构时，锁计数器加1，退出该块结构即执行monitorexit指令使锁计数器减1，直到锁计数器为0及释放锁

#### 保证可见性的原理

基于happens-bedore原则，即管程锁定规则，锁的unlock先于同一个锁的lock操作，即另一个线程获得锁必须等待当前线程释放锁

### 3、锁的优化

