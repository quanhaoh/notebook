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

