# 1. 什么是Thymeleaf
Thymeleaf是一款用于渲染XML/XHTML/HTML5内容的模板引擎。类似JSP，Velocity，FreeMaker等，它也可以轻易的与Spring MVC等Web框架进行集成作为Web应用的模板引擎。与其它模板引擎相比
# 2. 特点
通过特定语法对html标签做渲染
Thymeleaf最大的特点是能够直接在浏览器中打开并正确显示模板页面，而不需要启动整个Web应用
# 3. 优点
## 3.1. 即时预览性
Thymeleaf 在有网络和无网络的环境下皆可运行，即它可以让美工在浏览器查看页面的静态效果，也可以让程序员在服务器查看带数据的动态页面效果。这是由于它支持 html 原型，然后在 html 标签里增加额外的属性来达到模板+数据的展示方式。浏览器解释 html 时会忽略未定义的标签属性，所以 thymeleaf 的模板可以静态地运行；当有数据返回到页面时，Thymeleaf 标签会动态地替换掉静态内容，使页面动态显示。
## 3.2. 开箱即用性
Thymeleaf 开箱即用的特性。它提供标准和spring标准两种方言，可以直接套用模板实现JSTL、 OGNL表达式效果，避免每天套模板、该jstl、改标签的困扰。同时开发人员也可以扩展和创建自定义的方言。
## 3.3. 完美兼容性
Thymeleaf 提供spring标准方言和一个与 SpringMVC 完美集成的可选模块，可以快速的实现表单绑定、属性编辑器、国际化等功能
# 4. 标准表达式语法
## 4.1. 内置语法
形式：
```
${#strings.contains()}
```
## 4.2. 变量表达式
Spring EL表达式(在Spring术语中也叫model attributes)
格式：
```
${session.user.name}  
```

它们将以HTML标签的一个属性来表示：
如：
```
<span th:text="${book.author.name}"> 
<li th:each="book : ${books}">
```
## 4.3. 选择或星号表达式
选择表达式很像变量表达式，不过它们用一个预先选择的对象来代替上下文变量容器(map)来执行，如下：
```
*{customer.name}
```

被指定的object由th:object属性定义：
```
    <div th:object="${book}">  
      ...  
      <span th:text="*{title}">...</span>  
      ...  
    </div>
```
## 4.4. 文字国际化表达式
文字国际化表达式允许我们从一个外部文件获取区域文字信息(.properties)，用Key索引Value，还可以提供一组参数(可选).
```
    #{main.title}  
    #{message.entrycreated(${entryId})}
```
可以在模板文件中找到这样的表达式代码：

```
    <table>  
      ...  
      <th th:text="#{header.address.city}">...</th>  
      <th th:text="#{header.address.country}">...</th>  
      ...  
    </table>
```
## 4.5. URL表达式
URL表达式指的是把一个有用的上下文或回话信息添加到URL，这个过程经常被叫做URL重写。
```
@{/order/list}
```
URL还可以设置参数：
```
@{/order/details(id=${orderId})}
```
相对路径：
```
@{../documents/report}  
```

让我们看这些表达式：
```
    <form th:action="@{/createOrder}">  
    <a href="main.html" th:href="@{/main}">
```
# 5. 几种常用的使用方法
## 5.1. 赋值、字符串拼接
```
 <p  th:text="${collect.description}">description</p>
 <span th:text="'Welcome to our application, ' + ${user.name} + '!'">
```
字符串拼接还有另外一种简洁的写法

```
<span th:text="|Welcome to our application, ${user.name}!|">
```
## 5.2. 条件判断 If/Unless
Thymeleaf中使用th:if和th:unless属性进行条件判断，下面的例子中，<a>标签只有在th:if中条件成立时才显示：

```
<a th:if="${myself=='yes'}" > </i> </a>
<a th:unless=${session.user != null} th:href="@{/login}" >Login</a>
```

th:unless于th:if恰好相反，只有表达式中的条件不成立，才会显示其内容。
也可以使用  (if) ? (then) : (else) 这种语法来判断显示的内容

## 5.3. for 循环
```
  <tr  th:each="collect,iterStat : ${collects}"> 
     <th scope="row" th:text="${collect.id}">1</th>
     <td >
        <img th:src="${collect.webLogo}"/>
     </td>
     <td th:text="${collect.url}">Mark</td>
     <td th:text="${collect.title}">Otto</td>
     <td th:text="${collect.description}">@mdo</td>
     <td th:text="${terStat.index}">index</td>
 </tr>
```
iterStat称作状态变量，属性有：

index:当前迭代对象的index（从0开始计算）

count: 当前迭代对象的index(从1开始计算)

size:被迭代对象的大小

current:当前迭代变量

even/odd:布尔值，当前循环是否是偶数/奇数（从0开始计算）

first:布尔值，当前循环是否是第一个

last:布尔值，当前循环是否是最后一个

## 5.4. URL
URL在Web应用模板中占据着十分重要的地位，需要特别注意的是Thymeleaf对于URL的处理是通过语法@{…}来处理的。
如果需要Thymeleaf对URL进行渲染，那么务必使用th:href，th:src等属性，下面是一个例子

```
<!-- Will produce 'http://localhost:8080/standard/unread' (plus rewriting) -->
 <a  th:href="@{/standard/{type}(type=${type})}">view</a>

<!-- Will produce '/gtvg/order/3/details' (plus rewriting) -->
<a href="details.html" th:href="@{/order/{orderId}/details(orderId=${o.id})}">view</a>
```
设置背景

```
<div th:style="'background:url(' + @{/<path-to-image>} + ');'"></div>
```
根据属性值改变背景
```

 <div class="media-object resource-card-image"  th:style="'background:url(' + @{(${collect.webLogo}=='' ? 'img/favicon.png' : ${collect.webLogo})} + ')'" ></div>
```
几点说明：

上例中URL最后的(orderId=${o.id}) 表示将括号内的内容作为URL参数处理，该语法避免使用字符串拼接，大大提高了可读性

@{...}表达式中可以通过{orderId}访问Context中的orderId变量

@{/order}是Context相关的相对路径，在渲染时会自动添加上当前Web应用的Context名字，假设context名字为app，那么结果应该是/app/order

## 5.5. 内联js
内联文本：[[…]]内联文本的表示方式，使用时，必须先用th:inline=”text/javascript/none”激活，th:inline可以在父级标签内使用，甚至作为body的标签。内联文本尽管比th:text的代码少，不利于原型显示。

```
<script th:inline="javascript">
/*<![CDATA[*/
...
var username = /*[[${sesion.user.name}]]*/ 'Sebastian';
var size = /*[[${size}]]*/ 0;
...
/*]]>*/
</script>
```
js附加代码：

```
/*[+
var msg = 'This is a working application';
+]*/
```
js移除代码：
```

/*[- */
var msg = 'This is a non-working template';
/* -]*/
```
## 5.6. 内嵌变量
为了模板更加易用，Thymeleaf还提供了一系列Utility对象（内置于Context中），可以通过#直接访问：

dates ：  java.util.Date的功能方法类。

calendars :  类似#dates，面向java.util.Calendar

numbers :  格式化数字的功能方法类

strings :  字符串对象的功能类，contains,startWiths,prepending/appending等等。

objects:  对objects的功能类操作。

bools:  对布尔值求值的功能方法。

arrays：对数组的功能类方法。

lists:   对lists功能类方法

sets

maps

# 6. 标准表达式语法
## 6.1. ${...} 
变量表达式Variable Expressions

变量表达式有丰富的内置方法，使其更强大，更方便。
变量表达式功能
```
一、可以获取对象的属性和方法
二、可以使用ctx，vars，locale，request，response，session，servletContext内置对象
三、可以使用dates，numbers，strings，objects，arrays，lists，sets，maps等内置方法（重点介绍）
```

常用的内置对象

- ctx ：上下文对象。
-  vars ：上下文变量。
- locale：上下文的语言环境。
- request：（仅在web上下文）的 HttpServletRequest 对象。
- response：（仅在web上下文）的 HttpServletResponse 对象。
- session：（仅在web上下文）的 HttpSession 对象。
- servletContext：（仅在web上下文）的 ServletContext 对象

这里以常用的Session举例，用户刊登成功后，会把用户信息放在Session中，Thymeleaf通过内置对象将值从session中获取。
```
// java 代码将用户名放在session中
session.setAttribute("userinfo",username);
// Thymeleaf通过内置对象直接获取
th:text="${session.userinfo}"
```

常用的内置方法

- strings：字符串格式化方法，常用的Java方法它都有。比如：equals，equalsIgnoreCase，length，trim，toUpperCase，toLowerCase，indexOf，substring，replace，startsWith，endsWith，contains，containsIgnoreCase等
- numbers：数值格式化方法，常用的方法有：formatDecimal等
- bools：布尔方法，常用的方法有：isTrue，isFalse等
- arrays：数组方法，常用的方法有：toArray，length，isEmpty，contains，containsAll等
- lists，sets：集合方法，常用的方法有：toList，size，isEmpty，contains，containsAll，sort等
- maps：对象方法，常用的方法有：size，isEmpty，containsKey，containsValue等
- dates：日期方法，常用的方法有：format，year，month，hour，createNow等
## 6.2. @{...}
链接表达式，Link URL Expressions

不管是静态资源的引用，form表单的请求，凡是链接都可以用@{...} 。这样可以动态获取项目路径，即便项目名变了，依然可以正常访问
#修改项目名，链接表达式会自动修改路径，避免资源文件找不到
server.context-path=/itdragon

链接表达式结构
```
无参：@{/xxx}
有参：@{/xxx(k1=v1,k2=v2)} 对应url结构：xxx?k1=v1&k2=v2
```

引入本地资源：@{/项目本地的资源路径}
引入外部资源：@{/webjars/资源在jar包中的路径}
## 6.3. #{...}
消息表达式，Message Expressions

消息表达式一般用于国际化的场景。结构：th:text="#{msg}" 
## 6.4. ~{...} 
代码块表达式，Fragment Expressions

~{...} 代码块表达式
支持两种语法结构
推荐：`~{templatename::fragmentname}`

支持：`~{templatename::#id}`

```
templatename：模版名，Thymeleaf会根据模版名解析完整路径：/resources/templates/templatename.html，要注意文件的路径。
fragmentname：片段名，Thymeleaf通过th:fragment声明定义代码块，即：th:fragment="fragmentname"
id：HTML的id选择器，使用时要在前面加上#号，不支持class选择器。
```

代码块表达式的使用
代码块表达式需要配合th属性（th:insert，th:replace，th:include）一起使用。

```
th:insert：将代码块片段整个插入到使用了th:insert的HTML标签中，
th:replace：将代码块片段整个替换使用了th:replace的HTML标签中，
th:include：将代码块片段包含的内容插入到使用了th:include的HTML标签中，
```
## 6.5. *{...} 
选择变量表达式，Selection Variable Expressions