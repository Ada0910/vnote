# 1. EL表达式
## 1.1. EL的作用
JSP2.0要把html和css分离、要把html和javascript分离、要把Java脚本替换成标签。标签的好处是非Java人员都可以使用。
JSP2.0 – 纯标签页面，即：不包含<% … %>、<%! … %>，以及<%= … %>
EL（Expression Language）是一门表达式语言，它对应<%=…%>。我们知道在JSP中，表达式会被输出，所以EL表达式也会被输出。
## 1.2. EL的格式
格式：${…}
例如：${1 + 2}
## 1.3. 关闭EL
如果希望整个JSP忽略EL表达式，需要在page指令中指定isELIgnored=”true”。
如果希望忽略某个EL表达式，可以在EL表达式之前添加“\”，例如：\${1 + 2}。
## 1.4. EL运算符
运算符	说明	范例	结果
```
+	加	${17+5}	22
-	减	${17-5}	12
*	乘	${17*5}	85
/或div	除	${17/5}或${17 div 5}	3
%或mod	取余	${17%5}或${17 mod 5}	2
==或eq	等于	${5==5}或${5 eq 5}	true
!=或ne	不等于	${5!=5}或${5 ne 5}	false
<或lt	小于	${3<5}或${3 lt 5}	true
>或gt	大于	${3>5}或${3 gt 5}	false 
<=或le	小于等于	${3<=5}或${3 le 5}	true 
>=或ge	大于等于	${3>=5}或${3 ge 5}	false 
&&或and	并且	${true&&false}或${true and false}	false 
!或not	非	${!true}或${not true}	false
||或or	或者	${true||false}或${true or false}	true
empty	是否为空	${empty “”}，可以判断字符串、数据、集合的长度是否为0，为0返回true。empty还可以与not或!一起使用。${not empty “”}	true
```
## 1.5. EL不显示null
　　当EL表达式的值为null时，会在页面上显示空白，即什么都不显示。
## 1.6. EL表达式格式
先来了解一下EL表达式的格式！现在还不能演示它，因为需要学习了EL11个内置对象后才方便显示它。
（1）	操作List和数组：${list[0]}、${arr[0]}；
（2）操作bean的属性：${person.name}、${person[‘name’]}，对应person.getName()方法；
（3）操作Map的值：${map.key}、${map[‘key’]}，对应map.get(key)。
## 1.7. EL内置对象
EL一共11个内置对象，无需创建即可以使用。这11个内置对象中有10个是Map类型的，最后一个是pageContext对象。
```
	pageScope
	requestScope
	sessionScope
	applicationScope
	param；
	paramValues；
	header；
	headerValues；
	initParam；
	cookie；
	pageContext；
```
## 1.8. 域相关内置对象（重点）
域内置对象一共有四个：
```
	pageScope：${pageScope.name}等同与pageContext.getAttribute(“name”)；
	requestScope：${requestScope.name}等同与request.getAttribute(“name”)；
	sessionScoep： ${sessionScope.name}等同与session.getAttribute(“name”)；
	applicationScope：${applicationScope.name}等同与application.getAttribute(“name”)；
```
## 1.9. EL四大作用域 9个jsp对象有效范围 及 对应的类
```
java中request,session,application的作用范围
page，request，session，application四者的作用范围:
page的作用范围是当前页面；对应El表达式的pageScope
request的作用范围是页面与页面之间的传递就是请求请求结束则结束； 对应El表达式的requestScope
session的作用范围是直到关闭浏览器；对应El表达式的sessionScope
 application的作用范围是关闭服务器；对应El表达式的applicationScope
 application不是JAVA上的...是JSP中的对应java中的ServletContext类...
 
 它和page request session application response out config exception pageContext都是JSP中的9个内置对象...
    在后台用ServletContext存储的属性数据可jsp页面以用application对象获得..
    而且application的作用域是整个Tomcat启动的过程...
    例如: ServletContext.setAttribute("username",username);
    则在JSP网页中可以使用  application.getAttribute("username");来得到这个用户
    
application与pageContext都是jsp9个对象的成员，
    application是servletContext类的实例，  
    pageContext是PageContext类的实例，
    使用pageContext可以访问page request session application response out config exception范围的变量。
    意思就是在项目中jsp页面可以用pageContext调用到其它8个jsp内置对象的一切。

JSP共有以下9种基本内置组件：   
  request  用户端请求，此请求会包含来自GET/POST请求的参数   对应HttpServletRequest类 
  response     网页传回用户端的回应                   对应HttpServletResponse类  
  pageContext  网页的属性是在这里管理              对应PageContext类
  session      与请求有关的会话期                    对应HttpSession类
  application  servlet正在执行的内容               对应ServletContext类
  out          用来传送回应的输出                    对应JspWrite类
  config       servlet的构架部件                 对应ServletConfig类
  page         JSP网页本身                      对应this
  exception    针对错误网页，未捕捉的例外                   对应Throwable接口

```
# 2. 表达式语言（EL）的隐式对象及其作用。 
EL的隐式对象包括
pageContext、initParam（访问上下文参数）、param（访问请求参数）、paramValues、header（访问请求头）、headerValues、cookie（访问cookie）、applicationScope（访问application作用域）、sessionScope（访问session作用域）、requestScope（访问request作用域）、pageScope（访问page作用域）。

用法如下所示：

```
${pageContext.request.method}
${pageContext["request"]["method"]}
${pageContext.request["method"]}
${pageContext["request"].method}
${initParam.defaultEncoding}
${header["accept-language"]}
${headerValues["accept-language"][0]}
${cookie.jsessionid.value}
${sessionScope.loginUser.username}

```