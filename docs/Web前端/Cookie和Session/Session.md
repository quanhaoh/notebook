## Session

服务器端会话技术，将数据保存在服务器中，与Cookie相反

### 使用步骤

创建HttpSession对象

- HttpSession session = request.getSession()

传输Session数据

- session.setAttribute(String name, Object value)

使用HttpSession对象获取数据

- session.getAttribute(String name)

删除Session

- session.removeAttribute(String name)

### 原理

Session是依赖于Cookie实现的

![cookie与session的依赖关系](https://gitee.com/quanhaoh/blogImage/raw/master/img/cookie%E4%B8%8Esession%E7%9A%84%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB.png)

### 细节

1. 客户端关闭后，再次进行会话时Session是否相同？
   - 客户端关闭后，默认是不保持的，因为Cookie默认是会话结束就删除，因此Session也随之删除
   - 设置Cookie，键值为JSESSIONID，并设置存活时间
2. 服务器关闭后，再次进行会话时Session是否相同？
   - 不相同，但在实际开发中要保证数据不丢失，tomcat服务器会自动解决该问题：
     - Session的钝化：在服务器正常关闭前，将Session对象序列化到硬盘中
     - Session的活化：在服务器启动时，将Session文件转换为Session对象
3. Session销毁的情况
   - 服务器关闭
   - Session对象调用invalidate()
   - Session对象默认30min删除，可以在tomcat服务器配置文件中修改

### 特点

1. 存储数据在服务器
2. Session可以存储任意类型、任意大小的数据