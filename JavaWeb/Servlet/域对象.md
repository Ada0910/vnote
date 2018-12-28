# 1. 四大域对象
- servletContext(application域)
- session
- request
- PageContext(page域) 
# 2. 作用
- 保存数据  
- 获取数据 
- 用于数据共享

# 3. 作用范围
```
page域： 只能在当前jsp页面中使用（当前页面）

request域： 只能在同一个请求中使用（转发）

session域： 只能在同一个会话（session对象）中使用（私有的）

context域： 只能在同一个web应用中使用。（全局的）
```
- 四个域的作用域范围大小

PageContext （page域） < request < session < ServletContext（application域）

- Servlet中可以使用的是
request、session、application三个对象

- JSP中可以使用
pageContext、request、session、application四个域对象

# 4. 方法
```
setAttribute("name",Object) 保存数据

getAttribute("name")  获取数据

removeAttribute("name") 清除数据
```


