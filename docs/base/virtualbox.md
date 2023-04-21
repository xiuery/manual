### virtualbox
通过virtualbox做集群测试

---

#### Installation
https://www.virtualbox.org/wiki/Downloads

#### 安装centOS
```
# 安装增强功能
sudo mkdir /media/cdrom
sudo mount /dev/sr0 /media/cdrom
cd /media/cdrom
sudo yum install gcc kernel-devel kernel-headers dkms make bzip2
# 内核需要重启才能生效
reboot

sudo yum update
sudo sh ./VBoxLinuxAdditons.run
# 如果增强功能安装失败，比如报“找不到header”
cat /var/log/VBoxGuestAdditions.log
# 安装一个和Linux内核版本匹配的kernel-devel
sudo yum install -y "kernel-devel-uname-r == $(uname -r)"

# 虚拟机安装并使得其能够连接外网
# 设置网络：Host-Only/VirtualBox Host-Only Ethernet Adapter
# 查看ip
ip a
# 配置网络
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=staic
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s8
UUID=ebf8c895-2732-4428-bcee-393e1cb3f4a7
DEVICE=enp0s8
ONBOOT=yes
IPADDR=192.168.56.101
```

#### Links
- https://blog.csdn.net/Big_Blogger/article/details/75129833
- https://www.cnblogs.com/uqing/p/8160318.html