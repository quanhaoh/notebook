## SpringMVC与Structs2的区别

### 共同点

- 都是表现层框架，都是基于MVC模型编写
- 底层都离不开原始ServletAPI
- 处理请求机制都是一个核心控制器

### 不同点

- SpringMVC的入口是Servlet，Structs的入口是Filter
- SpringMVC是基于方法设计的（单例），Structs基于类（多例）
- SpringMVC支持JSR303，处理ajax更方便