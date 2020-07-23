## Listener监听器

web三大组件之一

事件监听机制

- 事件：一件事情
- 事件源：事件发生的地方
- 监听器：一个对象
- 注册监听：将事件、事件源、监听器绑定在一起，当事件源上发生某个事件后，执行监听代码



### 实现类

ServletContextListener：监听ServletContext对象的创建和销毁

- void contextDestroyed(ServeletContextEvent sce)：对象销毁前执行
- void contextInialized(ServeletContextEvent sce)：对象创建后执行



### 实现步骤

1. 定义监听器类，实现ServletContextListener类
2. 复写方法
3. 配置
   - web.xml
   - 注解