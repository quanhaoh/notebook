## 解决jar包冲突

### 方式1：声明顺序优先原则

先声明则优先使用

### 方式2:：最短依赖路径优先原则

直接依赖比传递依赖路径近

- 直接依赖：直接导入的jar包，就是该项目的直接依赖包
- 间接依赖：通过某个直接依赖包间接导入的jar包

同一个包下：

```java
// 优先使用2.0，因为路径更短
A -> B -> C -> X(1.0)
A -> D -> X(2.0)
```

### ==方式3：直接排除法==

排除某个直接依赖jar包下的依赖包时，使用<exclusions>标签

<exclusion>标签中排除的jar包可以不写版本号

```xml
<!-直接排除spring-core包-->
<dependency>
	<groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>4.2.4RELEASE</version>
    <exclusions>
    	<exclusion>
        	<groupId>org.springframework</groupId>
    		<artifactId>spring-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

