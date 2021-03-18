### centos8
centos8 + zerotier + mysql8 + docker + gilab + jenkins

#### 刻录镜像
- 下载系统ISO
```
# 请选择dvd, boot版本没试过
http://isoredirect.centos.org/centos/8/isos/x86_64/
```

- 刻录U盘
```
# 用UltraISO无法启动安装，可能是镜像名LABEL错误
# 改了也不行：linuxefi  /images/pxeboot/vmlinuz  inst.stage2=hd:LABEL=CentOS\x207\x20x\86_64 rd.live.check quiet
# 报错：dracut:/#
# 改用rufus刻录，地址：
http://rufus.ie/
```

- U盘启动安装
```
# 1.开机F12
# 2.Install CentOS Linux
# 3.语言：英文
# 4.网络：启用，并修改Host Name
# 5.Instation Source: 
mirrors.aliyun.com/centos/8/BaseOS/x86_64/os
mirrors.huaweiicloud.com/centos/8/BaseOS/x86_64/os
mirrors.tsinghua.edu.cn/centos/8/BaseOS/x86_64/os
# 6.Instation Destination:
**** 
/boot           – 2 GB (标准分区    ext文件系统)：启动文件
/boot/efi       – 2 GB (标准分区    ext4 文件系统)：
Swap            – 8 GB (           Swap 文件系统)：
/               – 100 GB (LVM      xfs 文件系统)：存放系统文件
/home           – 200 GB (LVM      xfs 文件系统)：存放用户文件
/var            – 100 GB (LVM      xfs 文件系统)：
# 7.安装完成，拔掉U盘，重启系统
```


#### zerotier安装
```
# 安装
curl -s https://install.zerotier.com | sudo bash
zerotier-cli info
zerotier-cli join af78bf943641978b
zerotier-cli listnetworks
```


#### mysql8安装：8.0.23-1.el8.x86_64
```
# 下载安装文件
wget https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-community-client-8.0.23-1.el8.x86_64.rpm
wget https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-community-client-plugins-8.0.23-1.el8.x86_64.rpm
wget https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-community-common-8.0.23-1.el8.x86_64.rpm
wget https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-community-libs-8.0.23-1.el8.x86_64.rpm
wget https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-community-server-8.0.23-1.el8.x86_64.rpm

# 安装依赖
yum install -y perl
yum install -y net-tools

# 依次安装
rpm -ivh mysql-community-common-8.0.23-1.el8.x86_64.rpm
rpm -ivh mysql-community-client-plugins-8.0.23-1.el8.x86_64.rpm
rpm -ivh mysql-community-libs-8.0.23-1.el8.x86_64.rpm
rpm -ivh mysql-community-client-8.0.23-1.el8.x86_64.rpm
rpm -ivh mysql-community-server-8.0.23-1.el8.x86_64.rpm

# 启动、重启、停止服务
systemctl start mysqld
systemctl restart mysqld
systemctl stop mysqld

# 查看初始化root密码
cat /var/log/mysqld.log | grep password

# 安全设置
mysql_secure_installation
# Change the password for root?             Y
# Remove anonymous users?                   Y
# Disallow root login remotely?             N
# Remove test database and access to it?    Y
# Reload privilege tables now?              Y

# 重置密码
mysql> alter user 'root'@'localhost' identified by 'newpassword';

# 添加用户并授权
mysql> create user xiuery@'%' identified by 'xiuery-pwd';
mysql> grant select,update,delete on xs_user.* to xiuery;

# 允许远程登录
#   这里安全与数据库组合用：设置root只允许本地连接
firewall-cmd --permanent --add-port=3306/tcp
firewall-cmd --reload
firewall-cmd --list-ports

# 允许Navicat连接
mysql> select user,host,plugin,authentication_string from user;
mysql> alter user 'xiuery'@'localhost' identified with mysql_native_password by 'xiuery-pwd';
```