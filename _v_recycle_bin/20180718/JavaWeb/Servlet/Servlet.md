一、Servlet简介
（服务端小程序）必须继承HttpServlet
常见方法：
doGet:
doPost:
doPut:
doDelete:

二、Servlet的配置
3.0以后的版本：
a.使用@WebServlet注解进行配置
b.通过在web.xml文件中进行配置
三、load-on-startup Servlet
四、Servlet配置参数
（1）Servlet配置参数会通过Servlet对象完成，提供一下方法：
java.lang.String getInitParameter(java.lang.String name):用于获取初始化参数
五、使用Servlet作为控制层
 