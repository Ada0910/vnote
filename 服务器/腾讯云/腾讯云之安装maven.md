# 1. 准备工作
- 访问maven的官网（[maven官网](http://maven.apache.org/download.cgi)）为下载maven对应的版本
![](_v_images/20201206154313248_1053.png =3810x)
# 2. 安装maven 
## 2.1. 解压
```
tar vxf apache-maven-3.6.3-bin.tar.gz(文件名)
```

## 2.2. 移动
```
mv apache-maven-3.6.3 /usr/local/maven3（移动相关的目录到/usr/local/maven3下，这个路径可以根据自己喜欢修改）
```

## 2.3. 打开配置文件
```
vi /etc/profile
```
- 修改环境变量， 在/etc/profile中添加以下几行
```
MAVEN_HOME=/usr/local/maven3（这个路径填刚才移动的路径）
export MAVEN_HOME
export PATH=${PATH}:${MAVEN_HOME}/bin
```


## 2.4. 使配置生效
```
source /etc/profile
```

## 2.5. 验证结果
```
mvn -v
```
![](_v_images/20201206154713297_22067.png =1340x)
