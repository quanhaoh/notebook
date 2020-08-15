## 获取servletContext

### 方式1：通过request对象获取

ServletContext context = request.getServletContext()

### 方式2：通过HttpServlet获取

ServletContext context =  this.getServletContext()



### 两种方式的效果是一样的