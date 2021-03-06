# 1. 常见技巧
## 1.1. 准确匹配
```
格式：“xxx关键字xxx”；
```

简单有效的方法就是在关键词上加上双引号, 这样搜索引擎只会返回和关键词完全吻合的搜索结果. 
在不加双引号的情况下,有的时候, 两个词中间加一个空格, 它会分别搜索两个词, 可能返回的结果不是我们想要的结果.

## 1.2. 排除关键字
```
格式：xxxx -xxx(排除的内容)
```
如果想要的不是自己想要的结果, 可以使用 - 这个减号即可对指定内容进行排除.

## 1.3. 用 OR (或)逻辑进行搜索 
```
格式：xxx or xxx
```
在默认搜索下, 搜索引擎会反馈所有和查询词汇相关的结果, 如果通过OR 搜索, 可以得到和两个关键词分别相关的结果, 而不仅仅是和两个关键词都同时相关的结果.

## 1.4. 同义词搜索  
```
格式： xxx ~ xxx
```
在未能准确判断关键词的情况下，你可以通过 ~ 进行同义词搜索。

## 1.5. 站内搜索 
站内搜索 
```
格式：site: 网址 关键字


```
在输入框输入 site: 网址 关键字 就会在输入的网址内进行站内关键字搜索

在两个数值之间进行搜索 
```
格式：1.. (空格)6
```
在两个数值之间进行搜索 ,数值之间的符号是两个英文的.加一个空格
## 1.6. 在网页标题, 链接和主体中搜索关键词 
## 1.7. 在网页标题, 链接和主体中搜索关键词 
```
格式：intitle/inurl/intext ：xxx

```

```
 你想在标题里面搜索超过2个词的时候，你可以使用“allintitle:” 
 格式：“allintitle: login password”。
```
标题是输入完 intitle 就有的备选选项, 索性就用这个了,

- 如果是想匹配标题关键字是 intitle: 

- 如果是主体的话是 intext: . 

- 如果想匹配网址链接 inurl: ,

```
格式一：inurl：xxx+关键词

格式二：关键词+ inurl：xxx
```
```
如果你想在网址里搜索多个关键词，你可以使用 “allinurl:”
语法。例如“allinurl: etc/passwd“
```

inurl是In-系指令中最强大的一个，换句话说，这个高级指令能够直接从网站的URL入手挖掘信息，只要略微了解普通网站的URL格式，就可以极具针对性地找到你所需要的资源--甚至隐藏内容。

　　inurl的应用范围十分广泛，在此我们仅抛砖引玉


### 1.7.1. 利用inurl搜图片--inurl:photo
搜索所有包含图片的关键词页面结果，如果说Google图象搜索侧重于展示图片，inurl搜索则让你在看到图片之前了解到页面大致的文字内容，更方便判断。
　　（1）利用inurl搜图片--inurl:photo，搜索所有包含图片的关键词页面结果，如果说Google图象搜索侧重于展示图片，inurl搜索则让你在看到图片之前了解到页面大致的文字内容，更方便判断。

　　利用这一指令，你往往能够找到关键词的组图内容(指令中的photo也可以替代为picture、image等)

　　例:搜索“乔丹经典”图片

```
　　输入:乔丹经典 inurl:photo，首个搜索结果上便提供了所有值得收藏的乔丹瞬间.
```


### 1.7.2. 利用inurl搜音乐--inurl:mp3
直接获得包含mp3音乐内容的页面搜索结果，Google中搜索音乐的另一有效方式(MP3可以替换为wma/ogg等)
　　（2）利用inurl搜音乐--inurl:mp3，直接获得包含mp3音乐内容的页面搜索结果，Google中搜索音乐的另一有效方式(MP3可以替换为wma/ogg等)

　　例:搜索T.A.T.U的经典歌曲“show me love”

```
　　输入:"show me love" inurl:mp3，直接找到这首歌的下载页面
```



### 1.7.3. 利用inurl搜软件--inurl:download
直接查找某个软件的下载页面，亦十分方便
　　（3）利用inurl搜软件--inurl:download，直接查找某个软件的下载页面，亦十分方便

　　例:搜索firefox下载页面

```
　　输入:firefox inurl:download，Google直接送上Mozilla的官方下载页面
```



　　（4）利用inurl当黑客--inurl/intitile/intext三个指令能够直接从网页结构来挖掘信息，很多被Google蜘蛛抓取的隐藏网页内容也无处遁形，从某种意义上，也为我们的“Google入侵”创造了条件。

　　有兴趣的话，你可以试试下面几个指令--纯属学术交流，不许乱用哦

　　intitle:"index of passwd"

　　inurl:service.pwd

　　site:xxxx.com intext:管理




## 1.8. 搜索相关网站 

## 1.9. 搜索相关网站 

```
格式：related: 网址
```
使用related: 网址 就会得到这个网址相关的结果.

# 2. 高级搜索
## 2.1. 搜索音乐
```
格式：“index of” + “mp3″ + “beatles” -html -htm -php
```

```
搜索指令解释:“index of”(目录) + “mp3″(音乐格式) + “beatles”(音乐人名) -html -htm -php(去除html/htm/php搜索结果--这些搜索词往往会带来干扰结果)

```
　　在上面的指令中，你可以将mp3换成WMA，AVI等格式，Beatles换成任意其他歌手或乐队名，获得类似的搜索结果。


```
　　如:“index of” + “wma″ + “savage garden” -html -htm -php
```

　　如:“index of” + “wma″ + “savage garden” -html -htm -php

　　这个搜索指令语句在Google.com中还可以简略为“index of/mp3”-playlist -html -lyrics beatles获得的搜索结果虽然不同，但是亦十分有效

　　值得一提的是，在Google.com英文搜索引擎中，你如果直接输入"index of /mp3"，将会获得508000个开放音乐文件目录搜索结果，里面保存着来自全球各地的音乐文件目录，在悠闲的时候，随便打开一个目录，随便下几首歌听，也未尝不是一个“开拓新世界”的好方法。

　　笔者就利用这个方法发现好几个开放下载的俄罗斯MP3 FTP服务器，里面的MP3数量的确惊人

　　歌词搜索除了寻找MP3音乐，你还可以利用Google的类似指令找到大量歌词数据例如，输入搜索指令语句:

　　“index of″ -playlist -html beatles lyric

　　你将找到网络上最完整的Beatels歌词库
## 2.2. 搜索电影
格式：
```
输入“imdb p”--获得搜索结果《Pulp Fiction》(低俗小说)
```
　　进入Google.cn，输入关键词imdb pulp fiction，你将直接看到最先排名的《低俗小说》影片介绍以及相关下载--在Google.cn中能够直接搜索到电骡的相关链接，这是英文版Google.com所不具有的。
　   或者：
　　使用任意浏览器打开谷歌搜索，然后输入你想要查找的影片，这里我就以正在上映的“猩球崛起3”作为搜索的目标吧，请记住，在查找资料的时候一定要以英文名字作为关键字查找，比如“猩球崛起3”我们应该写成“War for the Planet of the Apes ”因为电影的命名一般都是英文的，中文的很少，所以很可能输入中文就匹配不出来了，输入完关键字后再加上这串指令：-inurl:(htm|html|php|pls|txt) intitle:index.of “last modified” (mp4|wma|aac|avi)，如果是找预告片就在片面后面加上trailer，我想没人会为此找预告吧？然后我们就可以看到谷歌返回了一连串经过过滤的结果了。
## 2.3. Filetype
```

格式：xxxx　filetype:pdf
```
返回的就是包含xxx 这个关键词的所有pdf 文件

格式：xxxx　filetype:doc
```
# 3. 其他国家的网址
www.google.co.uk英国

www.google.fr法国

www.google.be比利时

www.google.de德国

www.google.dk丹麦

www.google.pt葡萄牙

www.google.es西班牙

www.google.ie爱尔兰

www.google.ch瑞士

www.google.pl波兰

www.google.it意大利

www.google.is冰岛

www.google.no挪威

www.google.se瑞典

www.google.fi芬兰

www.google.hu匈牙利

www.google.nl荷兰

www.google.lu卢森堡

www.google.ro罗马尼亚

www.google.gr希腊

www.google.com.ua乌克兰

www.google.ru俄罗斯

www.google.com.by白俄罗斯

# 4. 好玩篇
## 4.1. 生活类 

```
搜索语法：weather／time／sunrise／sundown+城市名（英语）
```
即时结果：返回各个城市的天气／所在时区的时间／日出时间／日落时间。


```
搜索语法：歌手名字(英语）+music/songs
```

即时结果：返回歌手的各首歌曲。


```
搜索语法：[货币一]+in+[货币二]
```
即时结果：返回货币一可以兑换多少货币二，还能给出


```
搜索语法：Set timer XX seconds/minutes/hours，XX表示具体的数字。
```
即时结果：设置XX秒/分/小时的计时器。

```
搜索语法：城市名+to+城市名+distance
```
即时结果：返回两个城市相距的距离。

```
搜索语法:what's my location/IP
```
即时结果：返回你所在的地址以及电脑的IP地址。

```
搜索语法：水果名称（英文）VS水果名称（英文）。
```
例如：durian vs banana，榴莲对比香蕉。你就可以看到两种水果组成成分的对比

```
搜索语法:追踪词汇的来源以及演变
```
```
搜索语法：Word etymology，word代表你想要搜索的单词。
```

```
Google Gravity 失去万有引力
Google Sphere旋转
```

# 5. 补充篇
## 5.1. 第一篇 
```
在搜索框上输入： “index of /” 　AVI
```
就是突破网站入口直接查找AVI的电影，还可以将AVI改为RMVB等等. 

```
在搜索框上输入： “index of/ ” 　inurl:lib
```

再按搜索你将进入许多图书馆，并且一定能下载自己喜欢的书籍。

```
在搜索框上输入： “index of /” 　cnki
```

再按搜索你就可以找到许多图书馆的CNKI、VIP、超星等入口！

```
在搜索框上输入：　“index of /” 　ppt
```

再按搜索你就可以突破网站入口下载powerpint作品！

```
在搜索框上输入： “index of /” 　mp3
```

再按搜索你就可以突破网站入口下载mp3、rm等影视作品！

```
在搜索框上输入：　“index of /” .s**
```

再按搜索你就可以突破网站入口下载flash作品！
```

在搜索框上输入： “index of /” 　要下载的软件名
```

再按搜索你就可以突破网站入口下载软件！

注意引号应是英文的！


## 5.2. 第二篇
用GOOgle看世界!!!只要你在GOOGLE里输入特殊的关键字,就可以搜到数千个摄象头的IP地址!通过他你就可以看到其所摄的实时影象!!
在google里输入
inurl:"viewerframe?mode="

随便打开一个,然后按提示装一个插件,就可以看到了!!!

## 5.3. 第三篇
　　这次我们能得到包含密码的文件。“site:virtualave.net”意思是只搜索 virutalave.net 的URL。virutalave.net是一个网络服务器提供商。

　　同样，我们可以搜索一些顶级域名，比如：.net .org .jp .in .gr

　　config.txt site:.jp

　　admin.txt site:.tw

　　搜索首页的目录

　　首页是非常有用的，它会提供给你许多有用的信息。

　　我们提交如下的形式：

　　"Index of /admin"

　　"Index of /secret"

　　"Index of /cgi-bin" site:.edu

　　你可以自己定义搜索的首页字符。这样就可以获得许多信息。

　　搜索特定的文件类型

　　比如你想指定一种文件的类型，可以提交如下形式：

　　file无效:.doc site:.mil classified

　　这个就是搜索军方的资料，你可以自定义搜索。
再提供一个第四篇

## 5.4. 特殊功能
- 查询电话号码
Google 的搜索栏中最新加入了电话号码和美国街区地址的查询信息。
个人如想查找这些列表，只要填写姓名，城市和省份。
如果该信息为众人所知，你就会在搜索结果页面的最上方看到搜索的电话和街区地址
你还可以通过以下任何一种方法找到该列表：
名字（或首位大写字母），姓，电话地区号
名字（或首位大写字母），姓，邮递区号
名字（或首位大写字母），姓，城市（可写州）
名字（或首位大写字母），姓，州
电话号码，包括区号
名字，城市，州
名字，邮递区号

- 查找 PDF 文件
现在 GOOGLE 的搜索结果中包括了 PDF 文件。尽管 PDF 文件不如 HTML 文件那么多，但他们经常具备一些其他文件不具备的高质量信息
为了显示一个搜索结果是 PDF 文件而不是网页， PDF 文件的标题开头显示蓝色文本。
这就是让你知道 ACRTOBAT READER 程序会启动来阅读文件
如果你的计算机没装有该程序，计算机会指导你去能免费下载该程序的网页。
使用 PDF 文件时，相关的网页快照会由“ TEXT VERSION ”代替，它是 PDF 文档的复制文件，该文件除去了所有格式化命令。
如果你在没有 PDF 链接的情况下想看一系列搜索结果，只要在搜索栏中打上 -inurldf 加上你的搜索条件。

- 股票报价
用 Google 查找股票和共有基金信息，只要输入一个或多个 NYSE ， NASDAQ ， AMEX 或
共有基金的股票行情自动收录机的代码，也可以输入在股市开户的公司名字。
如果 Google 识别出你查询的是股票或者共有基金，它回复的链接会直接连到高质量的金融信息提供者提供的股票和共有基金信息。
在你搜索结果的开头显示的是你查询的股市行情自动收录器的代码。如果你要查找一家公司的名字（比如， INTEL ），请查看“股票报价”在 Google 搜索结果的金融栏里会有那个公司的主页的链接（比如， WWW.INTEL.COM ）。
Google 是以质量为基础来选择和决定金融信息提供者的，包括的因素有下载速度，用户界面及其功能。

- 找找谁和你链接
有 些单词如果带有冒号就会有特殊的意思。比如 link ：操作员。查询 link:siteURL ，就会显示所有指向那个 URL 的网页。举例来说，链接 www.Google.com 会向你显示所有指向 GOOGLE 主页的网页。但这种方法不能与关键字查询联合使用。

- 查找站点
单词 site 后面如果接上冒号就能够将你的搜索限定到某个网站。具体做法是：在 c 搜索栏中使用 site:sampledomain.com 这个语法结构。比如，在斯坦福找申请信息，输入：
admission site:www.stanford.edu

- 查找字典释意
查找字典释意的方法是在搜索栏中输入你要查询的内容。在我们根据要求找到所有的字典释意都会标有下划线，位于搜索结果的上面，点击链接你会找到字典提供者根据要求给出的相关定义。 7 、用 GOOLGE 查找地图
想用 Google 查找街区地图，在 Google 搜索栏中输入美国街区地址，包括邮递区号或城市 / 州（比如 165 大学大街 PALO ALTO CA ）。通常情况下，街区地址和城市的名字就足够了。
当 Google 识别你的要求是查找地图，它会反馈给你有高质量地图提供者提供的链接，使你直接找到相关地图。我们是以质量为基础选择这些地图提供者。值得注意的是 Google 和使用的地图信息提供者没有任何关联。
