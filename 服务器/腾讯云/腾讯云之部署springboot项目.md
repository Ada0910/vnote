# 1. 服务到腾讯云上（Jar形式）
- 首先，我的项目是springboot项目，且是基于maven的，自带有Tomcat插件
```
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <!-- fork:如果没有该配置，这个devtools不会起作用，即应用不会restart -->
                    <fork>true</fork>
                </configuration>
            </plugin>
```

- 在这里要加上jar包，如图
![](_v_images/_1564217419_27139.png)

- 在maven插件这里，点击install，
![](_v_images/_1564217698_6667.png)

- 然后在target目录下就有一个jar包，
![](_v_images/_1564217785_215.png)

- 将jar包通过xftp（xftp这个东西好烦，老是要更新，然后各种问题）
![](_v_images/_1564217906_12600.png)

- 用xshell连接上云服务器上，cd到根目录下，运行
`java -jar xxx(jar的名称，后缀也要加上去)`
![](_v_images/_1564218013_21879.png)

- 至此，打开链接就可以到网站了
![](_v_images/_1564218055_23945.png)


# 2. 补充
在linux服务器上运行Jar文件时通常的方法是：
```
$ java -jar test.jar
```
这种方式特点是ssh窗口关闭时，程序中止运行.或者是运行时没法切出去执行其他任务，有没有办法让Jar在后台运行呢：

方法一：
```
$ nohup java -jar test.jar &
//nohup 意思是不挂断运行命令,当账户退出或终端关闭时,程序仍然运行
//当用 nohup 命令执行作业时，缺省情况下该作业的所有输出被重定向到nohup.out的文件中
//除非另外指定了输出文件。
```

方法二：
```
$ nohup java -jar test.jar >temp.txt &

```

//这种方法会把日志文件输入到你指定的文件中，没有则会自动创建
jobs命令和 fg命令：

```
$ jobs
//那么就会列出所有后台执行的作业，并且每个作业前面都有个编号。

```

//如果想将某个作业调回前台控制，只需要 fg + 编号即可。
```
$ fg 2
```
查看某端口占用的线程的pid
```
netstat -nlp |grep :8080
```
