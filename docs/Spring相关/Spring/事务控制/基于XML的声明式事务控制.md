## 基于xml的声明式事务控制

#### 1、配置事务管理器

包括注入数据源

```xml
<bean id="transactionManager" class="org.framwork.jdbc.datasource.DataSourceTansactionManager">
	<property name="dataSource" ref="dataSource"></property>
</bean>
```

#### 2、配置事务通知和事务属性

<tx:method>标签中的name属性值可以使用*通配符，如get\*表示所有以get开头的方法

```xml
<tx:advice id="txAdvice" transaction-manager="tansactionManager">
    <tx:attributes>
    	<tx:method name="methodName"></tx:method>
    </tx:attributes>
</tx:advice>
```

#### 3、配置AOP中的通用切入点表达式、建立事务通知和切入点表达式的对应关系

```xml
<aop:config>
	<aop:pointcut id="p1" expression="execution(...)"></aop:pointcut>
    <aop:advisor advice-ref="txAdivice" pointcut-ref="p1"></aop:advisor>
</aop:config>
```



