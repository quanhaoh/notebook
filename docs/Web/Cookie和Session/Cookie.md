## Cookie

客户端会话技术，将数据保存到客户端

### 使用步骤

创建Cookie对象，绑定数据

- Cookie cookie = new Cookie(String name, String value)

发送Cookie对象

- response.addCookie(Cookie cookie)

获取Cookie，获得贡献的数据

- Cookie[] request.getCookie()

### 细节

#### 1 一次发送多个Cookie

创建多个Cookie对象，调用多次addCookie方法

#### 2 使用时间

默认情况下Cookie只存在于浏览器内存中，关闭即删除

持久化存储：cookie.setMaxAge(int seconds)

- seconds为正数：将Cookie数据写入硬盘，持久化存储以秒为单位
- seconds为负数：默认值，即只存在浏览器内存中
- seconds为0：删除客户端浏览器的Cookie

#### 3 Cookie存取中文数据

在tomcat8之前不可以存中文：需将中文转码，一般采用URL编码

tomcat8之后可以

#### 4 Cookie的共享范围

1. 在同一个tomcat服务器中，默认情况下无法在多个web项目中共享，只能在当前的虚拟目录下获取

   设置共享范围：

   - setPath(String path)
   - path即共享的范围，让当前服务器下的所有web项目获取该Cookie：cookie.setPath("/")

2. 在不同tomcat服务器中，设置Cookie共享范围
   - setDomain(String path)
   - 设置path为某级域名，则其低级域名服务器间可以共享Cookie
   - cookie.setDOmain(".baidu.com")，这则new.baidu.com和tieba.baidu.com间可以共享

### Cookie的特点

1. 存储数据在客户端
2. Cookie的大小限制为40kb，同一域名下的Cookie数限制在20以内
3. Cookie一般用于存取少量不敏感的数据
4. 在==不登陆==的情况下，实现用户身份的识别，实现定制服务