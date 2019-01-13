# 1. 安装步骤
- 首先，我们检测一下系统中是否已安装mysql的相关服务 
```
命令： rpm -qa | grep mysql，无输出则证明未安装 
```

- 然后yum检测查找系统自带的mysql安装文件 
CentOS7的yum源中未找到mysql服务。所以，我们要先下载mysql的repo源。
```
下载命令：wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm 
```

- 下载完成，接下来我们安装
```
mysql-community-release-el7-5.noarch.rpm包 
安装命令：sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm 
```


- 安装mysql-community-release-el7-5.noarch.rpm包完成，安装这个包后，会获得两个mysql的yum repo源：/etc/yum.repos.d/mysql-community.repo，/etc/yum.repos.d/mysql-community-source.repo.
```
开始安装mysql，命令sudo yum install mysql-server 
```
- 按步骤安装完就可以了，成功安装之后重启mysql服务 
```
重启命令：service mysqld restart
```
- 初次安装mysql是root账户是没有密码的 
设置密码的方法，依次输入命令并回车，然后重启即可

```
mysql -u root

mysql> set password for ‘root’@‘localhost’ = password('mypasswd');

mysql> exit
```
- 重启服务：

```
service mysqld restart
```
# 2. 登录及忘记修改密码
- 创建root管理员：
```
#mysqladmin -u root password 666666
```

- 登录：
```
mysql -u root -p
```

- 如果忘记密码，则执行以下代码来修改密码

```
service mysqld stop
mysqld_safe --user=root --skip-grant-tables
mysql -u root
use mysql
update user set password=password("666666") where user="root";
flush privileges;
```

- 开放3306端口(可以不用设置)
```
$ sudo vim /etc/sysconfig/iptables
```
添加以下内容：
```
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
```

- 修改权限可以使其他机器登录：
```
﻿mysql>mysql -u root -p  //这样应该可以进入MySQL服务器 

mysql>grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
//赋予任何主机访问数据的权限 （此处'%'很重要，代表任意ip都可以远程连接，而默认的root用户只能“localhost” 所以才会报 “ is not allowed to connect to this MySQL server” 无法连接数据库！）

mysql>FLUSH PRIVILEGES //修改生效 

mysql>EXIT //退出MySQL服务器
```
# 3. 附录
## 3.1. 安装客户端和服务器端

#确认mysql是否已安装：    
`yum list installed mysql`

`rpm -qa | grep mysql
`
#查看是否有安装包：
`yum list mysql`

#安装mysql客户端：
`yum install mysql`

#安装mysql 服务器端：
`yum install mysql-server`
`yum install mysql-devel`

## 3.2. 启动、停止设置
#数据库字符集设置
#mysql配置文件/etc/my.cnf中加入
`default-character-set=utf8`

#启动mysql服务：
`service mysqld start`
#或者
`/etc/init.d/mysqld start`

#设置开机启动：
`chkconfig --add mysqld`

#查看开机启动设置是否成功
`chkconfig --list | grep mysql*`
mysqld 0:关闭 1:关闭 2:启用 3:启用 4:启用 5:启用 6:关闭

#停止mysql服务：
`service mysqld stop`

## 3.3. 远程访问设置
#开放防火墙的端口号
#mysql增加权限：
#mysql库中的user表新增一条记录host为“%”，user为“root”。

`use mysql;
UPDATE user SET Host = '%' WHERE User = 'root' LIMIT 1;`
#%表示允许所有的ip访问
## 3.4. mysql的几个重要目录
#(a)数据库目录
`/var/lib/mysql/`
#(b)配置文件
`/usr/share /mysql（mysql.server命令及配置文件）`
#(c)相关命令
`/usr/bin（mysqladmin mysqldump等命令）`
#(d)启动脚本
`/etc/rc.d/init.d/（启动脚本文件mysql的目录）`
# 4. yum安装的mysql默认目录
## 4.1. 创建新目录
#数据目录设置为 /home/data
`mkdir -p /home/data`
## 4.2. 把MySQL服务进程停掉：
`mysqladmin -u root -p shutdown`

## 4.3. 把/var/lib/mysql 整个目录移到/home/data
　　`mv /var/lib/mysql　/home/data/`
　　

## 4.4. 修改配置文件 my.cnf
#假如/etc/目录下没有my.cnf配置文档，请到/usr/share/mysql/下找到*.cnf文档
#拷贝其中一个到/etc/并改名为my.cnf)中。命令如下：
```
cp /usr/share/mysql/my.cnf　/etc/my.cnf
vi　 my.cnf　　 
[mysqld]
port　　　= 3306
socket　 = /home/data/mysql/mysql.sock　　　#修改socket参数
```
## 4.5. 修改MySQL启动脚本/etc/init.d/mysql
```
`vi　/etc/init.d/mysql
datadir=/home/data/mysql　　 #修改datadir数据目录的位置
#做一个mysql.sock 链接：
ln -s /home/data/mysql/mysql.sock /var/lib/mysql/mysql.sock`
```
## 4.6. 检查相关目录的属主和权限。
`chown -R mysql:mysql /home/data/mysql/　 #设置数据库的归属为mysql `
## 4.7. 重新启动MySQL服务
`/etc/init.d/mysql　start`