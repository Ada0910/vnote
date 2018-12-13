一、jsp四大基础语法
（1）jsp注释
<%-- 注释 --%>在HTML的源代码中不能够查看
（2）jsp的声明
<% ! 声明部分%>注意：不能够石宏abstract修饰声明部分的方法
（3）jsp的输出表达式
<% =表达式%>
（4）jsp脚本
<%脚本代码%>注意：脚本不能够使用private，public等访问控制修饰，也不可使用static修饰

二、jsp的3个编译指令
（1）page
（2）include
<%@include file=" "%>
（3）taglib

三、jsp的7个动作指令
（1）forward:
<jsp:forward page="{relativeURL|<%= expression%>}">
</jsp:forward>
（2）param:
（3）include:
<jsp:include page="" flush="true">
<jsp:param name="" value="">
</jsp:include>
（4）plugin:
（5）useBean:
<jsp:useBeam id="" class="" scope="page|request|session|application">
（6）setProperty:
<jsp:setProperty name="" proterty="" value="">
（7）getProperty:
<jsp:getProperty name="" proterty="" value="">
三、九个内置对象
1.application:
（1）可以让多个JSP、Servlet共享数据，但是通常是把Web应用的状态数据放入其中
（2）可以获取Web应用配置参数
2.config:
3.exception: 
4.out:
5.page:
6.pageContext:
7.request:
（1）获取请求头/请求参数
HttpServletRequest接口的实例：
（2）操作request范围的属性
setAttribute(String attName,Object attValue);将attValue设置成request范围的属性
Object getAttribute(String attName);获取request范围的属性
（3）执行forward和include
a.getRequestDispatcher("/a.jsp").include(request,response);
b.getRequestDispatcher("/a.jsp").forward(request,response);
请求的方式：
（a）GET方式的请求（传送的数据量少）
（b）POST方式的请求（传输的数据量大，安全性高）
8.response:
（1）response响应生成非字符响应
（2）重定向 sendRedirect("");
（3）在Cookie的生命限期里会一直存在客户端的机器里addCookie(Cookie cookie);
9.session:
session对象是HttpSession的实例

四、jsp2
1.自定义标签类
JSP自定义标签类：
a、如果标签类包含属性，每个属性都应该有对应的getter和setter方法
b、重写doTage（）方法，这个方法负责生成页面的内容