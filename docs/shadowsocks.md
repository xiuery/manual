### Shadowsocks
shadowsocks代理

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
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
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

