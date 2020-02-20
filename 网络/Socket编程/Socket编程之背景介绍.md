# 1. 概念
① Socket编程：通常所说的网络编程其实就是Socket编程。

# 2. 什么是socket？ 
Socket的字面意思是插槽、插座的意思。其实，Socket是应用层与TCP/IP协议族通信的中间软件抽象层，它是一组接口。 
Socket通常也称作“套接字”，用于描述IP地址和端口。应用程序两端通过“套接字”向网络发出请求或者应答网络请求。

Socket和ServerSocket类位于java.net包中。ServerSocket用于服务器端，Socket是建立网络连接时使用的。 
在连接成功时，应用程序两端都会产生一个Socket实例，操作这个实例，完成所需的会话。对于一个网络连接来说，套接字是平等的，并没有差别，不因为在服务器端或在客户端而产生不同级别。 
不管是Socket还是ServerSocket它们的工作都是通过SocketImpl类及其子类完成的。

# 3. Socket中的常用方法
java.net.Socket类继承与Object，有8个构造方法。其中三个使用最频繁的方法：

void accept()：用于获取连接到服务端的客户端对象，并且返回一个客户端的Socket对象实例。该方法是一个阻塞式方法。 
“阻塞”使程序运行暂时”停留”，直到一个会话产生，然后程序继续；通常”阻塞”是由循环产生的。

InputStream getInputStream()：该方法获得网络连接输入，同时返回一个InputStream对象实例，。

OutputStream getOutputStream()：该方法连接的另一端将得到输入，同时返回一个OutputStream对象实例。

　 注意：其中getInputStream和getOutputStream方法均会产生一个IOException，它必须被捕获，因为它们返回的流对象，通常都会被另一个流对象使用。
　 
# 4. C/S模型程序开发原理
　　 服务器Server：使用ServerSocket监听指定的端口，端口可以随意指定（由于1024以下的端口通常属于保留端口，在一些操作系统中不可以随意使用，所以建议使用大于1024的端口），等待客户连接请求，客户连接后，会话产生；在完成会话后，关闭连接。

　　 客户端Client：使用Socket对网络上某一个服务器的某一个端口发出连接请求，一旦连接成功，打开会话。会话完成后，关闭Socket。客户端不需要指定打开的端口，通常临时的、动态的分配一个大于1024的端口。