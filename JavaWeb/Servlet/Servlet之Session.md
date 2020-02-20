# 1. HttpSession
HttpSession 在****服务器****中，为浏览器创建独一无二的内存空间，在其中保存会话相关的信息。
## 1.1. 创建
- 某server端程序调用 HttpServletRequest.getSession(true)这样的语句时才被创建
## 1.2. 生命周期

- 有效范围当前会话，从浏览器的打开到浏览器的关闭
- 在第一次调用 request.getSession() 方法时，服务器会检查是否已经有对应的session,如果没有就在内存 中创建一个session并返回。
- 删除 

```
当一段时间内session没有被使用（默认为30分钟），则服务器会销毁该session。

如果服务器非正常关闭（强行关闭），没有到期的session也会跟着销毁。

如果调用session提供的invalidate（），可以立即销毁session。
```

- 注意：服务器正常关闭，再启动，Session对象会进行钝化和活化操作。同时如果服务器钝化的时间在session 默认销毁时间之内，则活化后session还是存在的。否则Session不存在。如果JavaBean 数据在session钝化时，没有实现Serializable 则当Session活化时，会消失

- session存放在哪里：服务器端的内存中。不过session可以通过特殊的方式做持久化管理