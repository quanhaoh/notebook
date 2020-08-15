## Spring的AOP实现

spring的AOP动态代理默认使用JDK的Porxy类，如果需要使用cglib库需要进行配置

```xml
<aop:aspectj-autoproxy proxy-target-class="true"/>
```

使用aspectjweaver第三方库：解析切入点表达式

### 基于xml的AOP配置

配置被代理对象bean

配置通知bean

配置AOP

- 配置切面：\<aop:aspect\>标签
  - name属性表示切面的唯一标识符
  - ref属性表示通知类的bean对象
- 前置通知：\<aop:before\>标签
  - method属性表示通知类中的前置通知方法
  - pointcut属性表示切入点方法

- 后置通知：\<aop:after-returning\>标签
- 异常通知：\<aop:after-throwing\>标签
- 最终通知：\<aop:after\>标签

```xml
<aop:config>
	<aop:aspect name = "" ref = "">
    	<aop:before method = "" pointcut = "execution(切入点表达式)"></aop:before>
        <aop:after-returning method = "" pointcut = "execution(切入点表达式)"></aop:after-returning>
        <aop:after-throwing method = "" pointcut = "execution(切入点表达式)"></aop:after-throwing>
        <aop:adter method = "" pointcut = "execution(切入点表达式)"></aop:adter>
    </aop:aspect>
</aop:config>
```

#### 切入点表达式

写法：访问修饰符 返回类型 切入点方法的全限定名称

通用写法：* 包名.通知类.\*(..)

- 表示通知类下的所有方法

xml配置中的简化写法：使用\<aop:porintcut\>标签

```xml
<aop:before method = "" proint-ref = "p1"></aop:before>
<aop:pointcut id="p1" expression = "execution(切入点表达式)"></aop:pointcut>
```

#### 环绕通知：\<aop:around\>标签

```xml
<aop:config>
	<aop:aspect name = "" ref = "">
        <aop:around method = "" pointcut = ""></aop:around>
    </aop:aspect>
</aop:config>
```

环绕通知方法必须明确执行切入点方法

- 使用ProceedingJoinPoint接口作为方法参数，Spring会自动提供实现类的对象
- 使用该对象的proceed()方法明确执行切入点方法

```java
public void aopAroud(ProceedingJoinPoint joinPoint) {
	Object[] args = joinPoint.getArgs();
	joinPoint.proceed(args);
}
```

### 基于注解的AOP配置

##### 开启Srping对注解AOP的支持

```xml
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

##### 配置切入点表达式

在切面类中创建切入点表达式方法，使用Pointcut注解

@Pointcut("expression=(execution(...))")

```java
@Pointcut("expression=(execution(...))")
public void p1(){}
```

##### 配置切面类

@Aspect

- 表明该类是切面类

@Before()

@AfterReturning

@AfterThrowing

@After

@Around

```java
// Before中的value属性指向切入点表达式方法名称
@Before("p1()")
public void aopBefore() {};
```

