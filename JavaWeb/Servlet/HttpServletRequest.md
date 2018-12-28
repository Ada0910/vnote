# 1. 概念
HttpServletRequest对象代表客户端的请求，当客户端通过HTTP协议访问服务器时，HTTP请求头中的所有信息都封装在这个对象中，开发人员通过这个对象的方法，可以获得客户的这些信息
# 2. 常用方法
```
    getRequestURL方法返回客户端发出请求时的完整URL。
　　getRequestURI方法返回请求行中的资源名部分。
　　getQueryString 方法返回请求行中的参数部分。
　　getPathInfo方法返回请求URL中的额外路径信息。额外路径信息是请求URL中的位于Servlet的路径之后和查询参数之前的内容，它以“/”开头。
　　getRemoteAddr方法返回发出请求的客户机的IP地址。
　　getRemoteHost方法返回发出请求的客户机的完整主机名。
　　getRemotePort方法返回客户机所使用的网络端口号。
　　getLocalAddr方法返回WEB服务器的IP地址。
　　getLocalName方法返回WEB服务器的主机名
```
# 3. 转发和重定向
- forward是容器中控制权的转向，是服务器请求资源，服务器直接访问目标地址的URL，把那个URL 的响应内容读取过来，然后把这些内容再发给浏览器，浏览器根本不知道服务器发送的内容是从哪儿来的，所以它的地址栏中还是原来的地址。
- 一个web资源收到客户端请求后，通知服务器去调用另外一个web资源进行处理，称之为请求转发/307。
- forward更加高效，所以在满足需要时尽量使用forward（通过调用RequestDispatcher对象的forward()方法，该对象可以通过ServletRequest对象的getRequestDispatcher()方法获得），并且这样也有助于隐藏实际的链接；



- redirect就是服务器端根据逻辑，发送一个状态码，告诉浏览器重新去请求那个地址，因此从浏览器的地址栏中可以看到跳转后的链接地址，很明显redirect无法访问到服务器保护起来资源，但是可以从一个网站redirect到其他网站。
- 一个web资源收到客户端请求后，通知浏览器去访问另外一个web资源进行处理，称之为请求重定向/302。
- 在有些情况下，比如需要访问一个其它服务器上的资源，则必须使用重定向（通过HttpServletResponse对象调用其sendRedirect()方法实现）。
