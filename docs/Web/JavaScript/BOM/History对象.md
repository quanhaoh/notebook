## History对象

即当前窗口访问过的历史记录，而不是整个浏览器的历史记录，是一个set集合

### 1. 使用方式

- window.history.方法();

- history.方法名();

### 2. 方法

#### back()：加载history列表中前一个页面

#### forward()：获取history列表中下一个页面

#### go(n)：获取history列表中某一个具体的url

- 若n是正数，则前进n个页面
- 若n是负数，则后退n个界面

### 3. 属性

#### length：返回当前窗口history列表中URL的数量

