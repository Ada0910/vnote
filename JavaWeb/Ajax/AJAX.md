# 1. ajax的概念
AJAX（Asynchronous Javascript And XML）翻译成中文就是“异步Javascript和XML”。即使用Javascript语言与服务器进行异步交互，传输的数据为XML（当然，传输的数据不只是XML）。AJAX还有一个最大的特点就是，当服务器响应时，不用刷新整个浏览器页面，而是可以局部刷新。这一特点给用户的感受是在不知不觉中完成请求和响应过程。
	 （1）与服务器异步交互；
	 （2）浏览器页面局部刷新；
# 2. 异步交互和同步交互
   （1）同步交互：客户端发出一个请求后，需要等待服务器响应结束后，才能发出第二个请求；
	 （2）异步交互：客户端发出一个请求后，无需等待服务器响应结束，就可以发出第二个请求
# 3. AJAX的优缺点
（1）优点：
	AJAX使用Javascript技术向服务器发送异步请求；
	AJAX无须刷新整个页面；
	因为服务器响应内容不再是整个页面，而是页面中的局部，所以AJAX性能高；
（2）缺点：
	AJAX并不适合所有场景，很多时候还是要使用同步交互；
	AJAX虽然提高了用户体验，但无形中向服务器发送的请求次数增多了，导致服务器压力增大；
	因为AJAX是在浏览器中使用Javascript技术完成的，所以还需要处理浏览器兼容性问题；
# 4. AJAX核心（XMLHttpRequest）
   其实AJAX就是在Javascript中多添加了一个对象：**XMLHttpRequest对象**。所有的异步交互都是使用XMLHttpRequest对象完成的。也就是说，我们只需要学习一个Javascript的新对象即可。注意，各个浏览器对XMLHttpRequest的支持也是不同的！大多数浏览器都支持DOM2规范，都可以使用：var xmlHttp = new XMLHttpRequest()来创建对象；但IE有所不同，IE5.5以及更早版本需要：var xmlHttp = new ActiveXObject(“Microsoft.XMLHTTP”)来创建对象；而IE6中需要：var xmlHttp = new ActiveXObject(“Msmxl2.XMLHTTP”)来创建对象；而IE7以及更新版本也支持DOM2规范。
# 5. AJAX第一例小结
	创建XMLHttpRequest对象；
	调用open()方法打开与服务器的连接；
	调用send()方法发送请求；
	为XMLHttpRequest对象指定onreadystatechange事件函数，这个函数会在XMLHttpRequest的1、2、3、4，四种状态时被调用；
XMLHttpRequest对象的5种状态：
	0：初始化未完成状态，只是创建了XMLHttpRequest对象，还未调用open()方法；
	1：请求已开始，open()方法已调用，但还没调用send()方法；
	2：请求发送完成状态，send()方法已调用；
	3：开始读取服务器响应；
	4：读取服务器响应结束。
通常我们只关心4状态。
# 6. AJAX第二例（用户名是否已被注册）
```
<script type="text/javascript">
function createXMLHttpRequest() {
	try {
		return new XMLHttpRequest();
	} catch (e) {
		try {
			return new ActiveXObject("Msxml2.XMLHTTP");
		} catch (e) {
			return new ActiveXObject("Microsoft.XMLHTTP");
		}
	}
}

function send() {
	var xmlHttp = createXMLHttpRequest();
	xmlHttp.onreadystatechange = function() {
		if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {
			if(xmlHttp.responseText == "true") {
				document.getElementById("error").innerHTML = "用户名已被注册！";
			} else {
				document.getElementById("error").innerHTML = "";
			}
		}
	};
	xmlHttp.open("POST", "/ajaxdemo1/BServlet", true);
	xmlHttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
	var username = document.getElementById("username").value;
	xmlHttp.send("username=" + username);
}
</script>
<h1>注册</h1>
<form action="" method="post">
用户名：<input id="username" type="text" name="username" onblur="send()"/><span id="error"></span><br/>
密　码：<input type="text" name="password"/><br/>
<input type="submit" value="注册"/>
</form>
```

registerServlet.Java
```
public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		
		String username = request.getParameter("username");

		if("itcast".equals(username)) {
			response.getWriter().print(true);
		} else {
			response.getWriter().print(false);
		}
	}
```
