# 1. 简介
Servlet是JavaWeb的三大组件之一，它属于动态资源。Servlet的作用是处理请求，服务器会把接收到的请求交给Servlet来处理，在Servlet中通常需要：

- 接收请求数据；
- 处理请求；
- 完成响应。
　　例如客户端发出登录请求，或者输出注册请求，这些请求都应该由Servlet来完成处理！Servlet需要我们自己来编写。（服务端小程序）必须继承HttpServlet
常见方法：

- doGet:
- doPost:
- doPut:
- doDelete:
## 1.1. 实现三种方式：
- 实现javax.servlet.Servlet接口；
- 继承javax.servlet.GenericServlet类；
- **继承javax.servlet.http.HttpServlet类；**（通常我们会去继承HttpServlet类来完成我们的Servlet）
# 2. 生命周期

- Servlet 通过调用 init () 方法进行初始化。----出生
init 方法被设计成只调用一次。它在第一次创建 Servlet 时被调用，在后续每次用户请求时不再调用
```
public void init() throws ServletException {
  // 初始化代码...
}
```

- Servlet 调用 service() 方法来处理客户端的请求。----服务
service() 方法是执行实际任务的主要方法。Servlet 容器（即 Web 服务器）调用 service() 方法来处理来自客户端（浏览器）的请求，并把格式化的响应写回给客户端。

每次服务器接收到一个 Servlet 请求时，服务器会产生一个新的线程并调用服务。service() 方法检查 HTTP 请求类型（GET、POST、PUT、DELETE 等），并在适当的时候调用 doGet、doPost、doPut，doDelete 等方法。
```public void service(ServletRequest request, 
                    ServletResponse response) 
      throws ServletException, IOException{
}
```

doGet() 方法
GET 请求来自于一个 URL 的正常请求，或者来自于一个未指定 METHOD 的 HTML 表单，它由 doGet() 方法处理。

```
public void doGet(HttpServletRequest request,
                  HttpServletResponse response)
    throws ServletException, IOException {
    // Servlet 代码
}
```

doPost() 方法
POST 请求来自于一个特别指定了 METHOD 为 POST 的 HTML 表单，它由 doPost() 方法处理。

```
public void doPost(HttpServletRequest request,
                   HttpServletResponse response)
    throws ServletException, IOException {
    // Servlet 代码
}
```

- Servlet 通过调用 destroy() 方法终止（结束）。----离去
- 
destroy() 方法只会被调用一次，在 Servlet 生命周期结束时被调用。destroy() 方法可以让您的 Servlet 关闭数据库连接、停止后台线程、把 Cookie 列表或点击计数器写入到磁盘，并执行其他类似的清理活动。

在调用 destroy() 方法之后，servlet 对象被标记为垃圾回收。destroy 方法定义如下所示：

```
  public void destroy() {
    // 终止化代码...
  }
```

- Servlet 是由 JVM 的垃圾回收器进行垃圾回收的

# 3. Servlet和CGI的区别
Servlet与CGI的区别在于Servlet处于服务器进程中，它通过多线程方式运行其service()方法，一个实例可以服务于多个请求，并且其实例一般不会销毁，而CGI对每个请求都产生新的进程，服务完成后就销毁，所以效率上低于Servlet。

# 4. Servlet的配置
3.0以后的版本：

-  使用@WebServlet注解进行配置
- .通过在web.xml文件中进行配置
# 5. load-on-startup Servlet
