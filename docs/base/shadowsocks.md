### Shadowsocks
shadowsocks代理

---

#### Installation
```
# pip安装：https://pypi.org/project/pip/#files
python3 setup.py install
pip install shadowsocks
```

#### Configuration
```
mkdir /usr/local/shadowsocks/shadowsocks.json
{
    "server":"0.0.0.0",
    "server_port":4000,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"password",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}

# server_port: 代理端口，客户端需要用到
# centos7开放端口：
# 开启
service firewalld start
# 重启
service firewalld restart
# 关闭
service firewalld stop
# 查看防火墙规则
firewall-cmd --list-all 
# 查询端口是否开放
firewall-cmd --query-port=8080/tcp
# 开放80端口
firewall-cmd --permanent --add-port=80/tcp
# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp
#重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload
```

#### Start
```
ssserver -c /usr/local/shadowsocks/shadowsocks.json -d start
ssserver -c /usr/local/shadowsocks/shadowsocks.json -d stop
ssserver -c /usr/local/shadowsocks/shadowsocks.json -d restart

# 启动异常（shadowsocks2.8.2）：libcrypto.so.1.1: undefined symbol: EVP_CIPHER_CTX_cleanup
# 问题是由于在openssl1.1.0版本中，废弃了EVP_CIPHER_CTX_cleanup函数
# 52、111行libcrypto.EVP_CIPHER_CTX_cleanup.argtypes = (c_void_p,)改为libcrypto.EVP_CIPHER_CTX_reset.argtypes = (c_void_p,)
```

#### 开机启动
```
echo 'ssserver -c /usr/local/shadowsocks/shadowsocks.json -d start' >> /etc/rc.local
```

