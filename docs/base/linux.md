### Linux
linux 常用操作

---

#### 常用命令
```
# 系统、内核信息
uname -a
lsb_release -a
cat /proc/cpuinfo
cat /proc/version

# 内存信息及插槽
dmidecode -t memory

# 环境变量
printenv

# 压缩解压
tar -zxvf test.tar.gz
tar -zcvf xs-manual.tar.xz xs-manual/
tar --exclude=xs-manual/dist -zcvf xs-manual.tar.xz xs-manual/
# 列出压缩包文件
tar -tzvf xs-manual.tar.xz

# 文件查找
sudo find / -name "*.out"

# 时间戳
date +%s
date -d "2019-05-01 00:00:00" +%s

# 文件上传、下载
sudo apt-get install lrzsz

# 截屏
apt-get install scrot
scrot /var/log/screen_shot/`date +%s`.png

# 文件恢复
sudo apt-get install extundelete
date -d "2019-05-01 00:00:00" +%s               # 转时间戳
sudo extundelete /dev/sda8 --after 1401634920 --restore-all
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

#### 用户创建
```
# 创建nologin用户
groupadd -r nginx
useradd -r -g nginx -s /sbin/nologin -d /home/nginx -M nginx

# 修改密码
passwd
```

#### 查看端口
```
# 端口
ss -lnt

# 查看占用端口的pid
lsof -i:80
ps -ef | grep pid#
netstat -tunlp | grep 3306
netstat -apn | grep 3306
```

#### 端口列表
| service        | service                                              | port	  |
|----------------|------------------------------------------------------|--------|
|                | [xiuery.com ](https://xiuery.com)                    | 80     |
| wordpress      | [wp.xiuery.com](https://wp.xiuery.com)               | 80     |
| ssh            | [ssh.xiuery.com](https://ssh.xiuery.com)             | 2222   |
| ssh            | [ssh2.xiuery.com](https://ssh2.xiuery.com)           | 9999   |
| archery        | [archery.xiuery.com](https://archery.xiuery.com)     | 9123   |
| manual         | [manual.xiuery.com](https://manual.xiuery.com)       | 8100   |
| filebrowser    | [file.xiuery.com](https://file.xiuery.com)           | 8200   |
| nexus          | [nexus.xiuery.com](https://nexus.xiuery.com)         | 8000   |
| docker         | [docker.xiuery.com](https://docker.xiuery.com)       | 8001   |
| harbor         | [harbor.xiuery.com](https://harbor.xiuery.com)       | 8002   |
| chrome         | [chrome.xiuery.com](https://chrome.xiuery.com)       | 7900   |
| chrome session | [chrome-ss.xiuery.com](https://chrome-ss.xiuery.com) | 4444   |
| metabase       | [mb.xiuery.com](https://mb.xiuery.com)               | 3000   |
| jupyter        | [jupyter.xiuery.com](https://jupyter.xiuery.com)     | 8300   |
| gitlab         | [gitlab.xiuery.com](https://gitlab.xiuery.com)       | 8929   |


#### 排序命令
```
# ls排序

# 按时间排序
ls -lt    ==> ll -t
ls -ltr   ==> ll -tr
# 按文件大小排序
ls -lhS   ==> ll -hS
ls -lhSr  ==> ll -hSr

# sort命令: sort -k 1 [-r] [-n]
-k 1: 按第一个参数 rss进行排
-r: 倒序
-n: 按数字来排序

# 对文件排序
sort test.txt
sort -r test.txt                        # 倒序
sort -n test.txt                        # 纯数字字符串

sort -r -u test.txt                     # 去重
sort -r -u test.txt -o ./result.txt     # 结果输出到文件
sort -r -u test.txt > ./result.txt      # 结果输出到文件

sort -t ':' -k 1 test.txt               # 分割按第一列排序

# 结果排序
ps -ef | sort -k2
ps -ef | sort -k2 -r
ps -ef | sort -k2 -rn
ps -ef | sort -k2 -rn | head -10                        # 取前10行
ps -ef | head -1;ps -ef | grep s                        # 取前10行,固定title行
ps -ef | head -1;ps -ef | sort -k2 -rn | head -10       # 取前10行,固定title行
```

#### 查找文件内容
```
# 文件查找
grep –e "正则表达式" 文件名
# 不区分大小写
grep –i "被查找的字符串" 文件名
# 匹配的行数
grep -c "被查找的字符串" 文件名
# 不匹配指定字符串的行
grep –v "被查找的字符串" 文件名
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

#### 服务自启动
```
# 服务开机自启
chkconfig nexus on/off
systemctl enable/disable docker
echo /usr/local/nginx/sbin/nginx >> /etc/rc.local

# 后台运行: nohup
nohup sh /usr/local/nexus/nexus-{version}/bin/nexus run &
nohup wssh --address=0.0.0.0 --port=8888 --log-file-prefix=/var/log/webssh/wssh.log &
```

#### 防火墙命令
```
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
firewall-cmd --list-services

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
```

#### [WebSSH](https://github.com/huashengdun/webssh)
```
# 安装包
pip3 install webssh

# 启动: 写入/etc/rc.local开机启动
nohup wssh --address=0.0.0.0 --port=8888 --log-file-prefix=/var/log/webssh/wssh.log &

# web访问: 实际改成域名
https://localhost:8888/#bgcolor=silver&fontsize=16
https://localhost:8888/#bgcolor=silver&fontsize=16&hostname=localhost&username=root
https://localhost:8888/#bgcolor=silver&fontsize=16&hostname=localhost&username=root&command=cd%20/opt
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

#### BBR: TCP拥塞控制的算法
- 安装
```
# 内核 >= 4.9
lsmod | grep bbr    # 结果中没有tcp_bbr执行下面命令
sudo modprobe tcp_bbr
echo "tcp_bbr" | sudo tee --append /etc/modules-load.d/modules.conf
echo "net.core.default_qdisc=fq" | sudo tee --append /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" | sudo tee --append /etc/sysctl.conf

# 保存生效
sudo sysctl -p
```

- 验证是否开启
```
# 结果都有bbr，则证明你的内核已开启BBR
sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control

# 有tcp_bbr模块即说明BBR已启动
lsmod | grep bbr
```

#### 更新系统内核（4.19、4.20）
```
sudo apt-get update
sudo apt-get upgrade

# 升级内核依赖libssl、linux-base
# libssl：http://security.ubuntu.com/ubuntu/ubuntu/pool/main/o/openssl/
wget http://security.ubuntu.com/ubuntu/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4.3_amd64.deb
sudo dpkg -i libssl1.1_1.1.0g-2ubuntu4.3_amd64.deb

# 安装内核: https://kernel.ubuntu.com/~kernel-ppa/mainline/
sudo dpkg -i *.deb
```
