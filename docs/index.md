### Linux
linux 常用操作

---

#### 查看系统信息
```
# 修改密码
passwd

# 查看占用端口的pid
lsof -i:80
ps -ef | grep pid#

# 文件查找
sudo find / -name "*.out"

# 文件上传、下载
sudo apt-get install lrzsz

# 文件恢复
sudo apt-get install extundelete  
date -d "2019-05-01 00:00:00" +%s               # 转时间戳
sudo extundelete /dev/sda8 --after 1401634920 --restore-all  

# scp命令
scp jdk1.8.tar.gz root@10.0.0.1:~               # 将本地文件拷贝到远程服务器的home目录
scp -r jdk1.8 root@10.0.0.1:~                   # 将本地文件夹拷贝到远程服务器的home目录
scp root@10.0.0.1:/usr/local/jdk1.8.tar.gz ./   # 将远程服务器的文件拷贝到本地当前目录
scp -r root@10.0.0.1:/usr/local/jdk1.8 ./       # 将远程服务器的文件夹拷贝到本地当前目录
scp -P 22222 jdk1.8.tar.gz root@10.0.0.1:~      # 指定的远程端口

# 查看安装列表
dpkg --list

# 系统信息
uname -a
lsb_release -a
cat /proc/cpuinfo

# 系统软件升级
sudo apt-get update         # 只升级软件
sudo apt-get upgrade        # 升级软件和系统
sudo apt-get dist-upgrade   # 升级软件和系统，会卸载一些依赖

# 删除
sudo apt-get --purge remove packge
```

#### ssh免密登录
```
# 免密登录的电脑上生成公密私密
ssh-keygen -t rsa

# 将生成.ssh目录下的id_rsa.pub拷贝到目标机器上
# /home/ubuntu/.ssh/authorized_keys

# ssh登录
ssh root@ip [-p port]
```

#### 更新系统内核（4.19、4.20）
```
sudo apt-get update 
sudo apt-get upgrade

# 升级内核依赖libssl、linux-base
# libssl：http://security.ubuntu.com/ubuntu/ubuntu/pool/main/o/openssl/
wget http://security.ubuntu.com/ubuntu/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4.3_amd64.deb
sudo dpkg -i libssl1.1_1.1.0g-2ubuntu4.3_amd64.deb

# 安装内核
# 选择内核版本https://kernel.ubuntu.com/~kernel-ppa/mainline/
sudo dpkg -i *.deb
```

#### [Java8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
```
# 下载解压到/usr/local：/usr/local/jdk1.8.0_211

# /etc/profile.d下新建java8.sh添加以下内容
export JAVA_HOME=/usr/local/jdk1.8.0_211
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin

# 生效profile
source /etc/profile

# 测试
java -version
```

#### [BBR](bbr.md)

#### [ZeroTier](zerotier.md)

#### [Shadowsocks](shadowsocks.md)

#### [SVN](svn.md)

#### [Maven](maven.md)

#### [Nexus](nexus.md)

#### [ZooKeeper](zookeeper.md)

#### [RocketMQ](rocketmq.md)

