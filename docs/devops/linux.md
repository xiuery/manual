### Linux
linux 常用操作

---

#### 常用命令
```
# 修改密码
passwd

# 查看占用端口的pid
lsof -i:80
ps -ef | grep pid#

# 压缩解压
tar -zxvf test.tar.gz
tar -zcvf xs-manual.tar.xz xs-manual/
tar --exclude=xs-manual/dist -zcvf xs-manual.tar.xz xs-manual/
tar -tzvf xs-manual.tar.xz          # 列出压缩包文件

# 文件查找
sudo find / -name "*.out"

# 文件上传、下载
sudo apt-get install lrzsz

# 时间戳
date +%s
date -d "2019-05-01 00:00:00" +%s

# 截屏
apt-get install scrot
scrot /var/log/screen_shot/`date +%s`.png

# 文件恢复
sudo apt-get install extundelete
date -d "2019-05-01 00:00:00" +%s               # 转时间戳
sudo extundelete /dev/sda8 --after 1401634920 --restore-all

# centos7开放端口：
# 防火墙命令帮助
firewall-cmd --help | less
# 开启
service firewalld start
# 重启
service firewalld restart
# 关闭
service firewalld stop
# 查看防火墙规则
firewall-cmd --list-all 
firewall-cmd --list-ports
# 查询端口是否开放
firewall-cmd --query-port=8080/tcp
# 永久开放80端口
firewall-cmd --permanent --add-port=80/tcp
# 临时打开端口，防火墙重启后失效
firewall-cmd --add-port=80/tcp
# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp
# 关闭端口
firewall-cmd --remove-port=80/tcp
#重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload

开启端口转发
firewall-cmd --add-masquerade
设置端口转发
firewall-cmd --add-forward-port=port=8080:proto=tcp:toport=80:toadddr=192.168.22.11
查看端口转发
firewall-cmd --query-forward-port
删除端口转发
firewall-cmd --remove-forward-port=port=8080:proto=tcp:toport=80:toadddr=192.168.22.11
关闭防火墙
systemctl stop firewalld.service
禁止防火墙开机启动
systemctl disable firewalld.service
启动防火墙
systemctl start firewalld.service
查看防火墙状态
firewall-cmd --state

# 查看安装列表
dpkg --list

# 系统信息
uname -a
lsb_release -a
cat /proc/cpuinfo
```

#### 查找文件内容
```
# 文件查找
grep –e “正则表达式” 文件名
# 不区分大小写
grep –i “被查找的字符串” 文件名
# 匹配的行数
grep -c “被查找的字符串” 文件名
# 不匹配指定字符串的行
grep –v “被查找的字符串” 文件名
# 目录下查找
find /var/log/ -type f -name "*.log" | xargs grep "119251535"
```

#### scp命令
```
scp jdk1.8.tar.gz root@10.0.0.1:~               # 将本地文件拷贝到远程服务器的home目录
scp -r jdk1.8 root@10.0.0.1:~                   # 将本地文件夹拷贝到远程服务器的home目录
scp root@10.0.0.1:/usr/local/jdk1.8.tar.gz ./   # 将远程服务器的文件拷贝到本地当前目录
scp -r root@10.0.0.1:/usr/local/jdk1.8 ./       # 将远程服务器的文件夹拷贝到本地当前目录
scp -P 22222 jdk1.8.tar.gz root@10.0.0.1:~      # 指定的远程端口
```

#### 安装包管理
```
# 安装列表
dpkg -l

# 系统软件升级
sudo apt-get update         # 只升级软件
sudo apt-get upgrade        # 升级软件和系统
sudo apt-get dist-upgrade   # 升级软件和系统，会卸载一些依赖

# 删除
sudo apt-get --purge remove package
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
