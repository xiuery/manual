### [Maven](http://maven.apache.org/install.html)
Java依赖包管理

#### Installation
```
# 下载https://mirrors.tuna.tsinghua.edu.cn/apache/maven/
# 解压到/usr/local/apache-maven-3.6.1

# /etc/profile.d下新建maven-3.6.1.sh添加以下内容
export MAVEN_HOME=/usr/local/apache-maven-3.6.1
export PATH=$PATH:$MAVEN_HOME/bin

# 生效profile
source /etc/profile
```

#### 测试
```
mvn -v
```
