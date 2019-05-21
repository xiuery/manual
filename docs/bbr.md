### BBR
TCP拥塞控制的算法

#### Installation
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

#### 验证是否开启
```
# 结果都有bbr，则证明你的内核已开启BBR
sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control

# 有tcp_bbr模块即说明BBR已启动
lsmod | grep bbr
```
