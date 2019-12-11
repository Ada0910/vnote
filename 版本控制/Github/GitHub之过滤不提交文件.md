# 1. git提交忽略不必要的文件或文件夹
## 1.1. .gitignore配置
对于经常使用Git的朋友来说，.gitignore配置一定不会陌生。废话不说多了，接下来就来说说这个.gitignore的使用

首先要强调一点，这个文件的完整文件名就是".gitignore"，注意最前面有个“.”

一般来说每个Git项目中都需要一个“.gitignore”文件，这个文件的作用就是告诉Git哪些文件不需要添加到版本管理中。实际项目中，很多文件都是不需要版本管理的，比如Python的.pyc文件和一些包含密码的配置文件等等。这个文件的内容是一些规则，Git会根据这些规则来判断是否将文件添加到版本控制中

# 2. 常见规则
下面我们看看常用的规则：

- /mtk/               过滤整个文件夹
- *.zip                过滤所有.zip文件
- /mtk/do.c         过滤某个具体文件

很简单吧，被过滤掉的文件就不会出现在git仓库中（gitlab或github）了，当然本地库中还有，只是push的时候不会上传。
需要注意的是，gitignore还可以指定要将哪些文件添加到版本管理中：

- !*.zip
- !/mtk/one.txt

唯一的区别就是规则开头多了一个感叹号，Git会将满足这类规则的文件添加到版本管理中。
为什么要有两种规则呢？想象一个场景：假如我们只需要管理/mtk/目录中的one.txt文件，这个目录中的其他文件都不需要管理，那么我们就需要使用：
```
1）/mtk/
2）!/mtk/one.txt
```

最后需要强调的一点是，如果你不慎在创建.gitignore文件之前就push了项目，那么即使你在.gitignore文件中写入新的过滤规则，这些规则也不会起作用，Git仍然会对所有文件进行版本管理。
简单来说，出现这种问题的原因就是Git已经开始管理这些文件了，所以你无法再通过过滤规则过滤它们。因此一定要养成在项目开始就创建.gitignore文件的习惯，否则一旦push，处理起来会非常麻烦

## 2.1. 配置示例
- 配置语法：
```
以斜杠“/”开头表示目录；
以星号“*”通配多个字符；
以问号“?”通配单个字符
以方括号“[]”包含单个字符的匹配列表；
以叹号“!”表示不忽略(跟踪)匹配到的文件或目录；
```

此外，git 对于 .ignore 配置文件是按行从上到下进行规则匹配的，意味着如果前面的规则匹配的范围更大，则后面的规则将不会生效；

- 示例说明
- - 规则：fd1/*
说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；

- - 规则：/fd1/*
说明：忽略根目录下的 /fd1/ 目录的全部内容；

- - 规则：
/*
!.gitignore
!/fw/bin/
!/fw/sf/
说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；

# 3. Git忽略规则及.gitignore规则不生效的解决办法
在git中如果想忽略掉某个文件，不让这个文件提交到版本库中，可以使用修改根目录中 .gitignore 文件的方法（如无，则需自己手工建立此文件）。这个文件每一行保存了一个匹配的规则例如：
```
# 4. 此为注释 – 将被 Git 忽略
*.a # 忽略所有 .a 结尾的文件
!lib.a # 但 lib.a 除外
/TODO # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/ # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```
　　规则很简单，不做过多解释，但是有时候在项目开发过程中，突然心血来潮想把某些目录或文件加入忽略规则，按照上述方法定义后发现并未生效，原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交：

```
git rm -r --cached .（或者git rm -r --cached dir（要删除的目录））
git add .
git commit -m 'update .gitignore'

```
注意：
　　不要误解了 .gitignore 文件的用途，该文件只能作用于 Untracked Files，也就是那些从来没有被 Git 记录过的文件（自添加以后，从未 add 及 commit 过的文件）
　
# 4. 工程文件示例
```
*.md linguist-language=Java 
*.yml linguist-language=Java 
*.html linguist-language=Java 
*.js linguist-language=Java 
*.xml linguist-language=Java
*.css linguist-language=Java 
*.sql linguist-language=Java
*.uml linguist-language=Java 
*.cmd linguist-language=Java 

build 
.idea
/target/
*.gitattributes;
*.gitignore;
*.hprof;
*.idea;
*.iml;
*.mvn;
*.pyc;
*.pyo;
*.rbc;
*.yarb;
*~;.DS_Store;
.git;
.hg;
.svn;
CVS;__pycache__;
_svn;
mvnw;
mvnw.*;
vssver.scc;
vssver2.scc;

```