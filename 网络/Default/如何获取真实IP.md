# 1. 定义
- 在JSP里，获取客户端的IP地址的方法是：`request.getRemoteAddr()`，这种方法在大部分情况下都是有效的。但是在通过了 Apache，Nagix等反向代理软件就不能获取到客户端的真实IP地址了。如果使用了反向代理软件，用` request.getRemoteAddr()`方法获取的IP地址是：127.0.0.1或 192.168.1.110，而并不是客户端的真实IP。

 - 经过代理以后，由于在客户端和服务之间增加了中间层，因此服务器无法直接拿到客户端的 IP，服务器端应用也无法直接通过转发请求的地址返回给客户端。但是在转发请求的HTTP头信息中，增加了X-FORWARDED-FOR信息。用以跟踪原有的客户端 IP地址和原来客户端请求的服务器地址。

- 举例来说，当我们访问口碑网首页hangzhou.jsp时，其实并不是我们浏览器真正访问到了服务器上的hangzhou.jsp 文件，而是先由代理服务器Nagix去访问hagnzhou.jsp ，代理服务器再将访问到的结果返回给我们的浏览器，因为是代理服务器去访问hangzhou.jsp的，所以hangzhou.jsp中通过 request.getRemoteAddr()的方法获取的IP实际上是代理服务器的地址，并不是客户端的IP地址。
# 2. 获取真实的IP
- 获得客户端真实IP地址的方法一：
```

public String getRemortIP(HttpServletRequest request) {
　　      if (request.getHeader("x-forwarded-for") == null) {
　　            return request.getRemoteAddr();
　　      }
　　      return request.getHeader("x-forwarded-for");
　　}
```

- 获得客户端真实IP地址的方法二

```
public String getIpAddr(HttpServletRequest request) {
　　      String ip = request.getHeader("x-forwarded-for");
　　      if(ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
　　            ip = request.getHeader("Proxy-Client-IP");
　　      }
　　      if(ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
　　            ip = request.getHeader("WL-Proxy-Client-IP");
　　      }
　　      if(ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
　　            ip = request.getRemoteAddr();
　　      }
　　      return ip;
　　}
```