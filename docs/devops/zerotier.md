### [ZeroTier](http://www.zerotier.com/download.shtml)
内网穿透

---

#### Installation
```
curl -s https://install.zerotier.com/ | sudo bash
```

#### Start
```
# 加入、离开网络
sudo zerotier-cli info
sudo zerotier-cli info listnetworks
sudo zerotier-cli join network
sudo zerotier-cli leave network
```

#### Re-Installation
```
# 重装注意删除配置文件
sudo rm -rf /var/lib/zerotier-one
```

