### Nginx
web,反向代理,负载均衡

---

#### 创建用户
```
groupadd -r nginx
useradd -r -g nginx -s/sbin/nologin -d /home/nginx -M nginx
```

#### 安装依赖
```
dpkg -l | grep pcre
sudo apt-get install openssl libssl-dev
sudo apt-get install libpcre3 libpcre3-dev
sudo apt-get install zlib1g-dev
sudo apt-get install libxml2-dev libxslt-dev
sudo apt-get install libperl-dev

# 注意libperl-dev,到usr/lib建立libperl.so软连接:
perl -V
sudo find / -name "*libperl.so*"
ln -s /usr/lib/x86_64-linux-gnu/libperl.so.5.22.1 /usr/lib/libperl.so
```

#### 编译安装
```
# 创建安装目录
mkdir /usr/local/nginx

# 下载解压
cd /usr/local/src
wget http://nginx.org/download/nginx-1.16.0.tar.gz
tar -zxvf nginx-1.16.0.tar.gz
cd nginx-1.16.0

# configure
./configure \
--prefix=/usr/local/nginx \
--user=nginx \
--group=nginx \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--pid-path=/var/run/nginx.pid  \
--lock-path=/usr/local/nginx/nginx.lock \
--with-http_ssl_module \
--with-http_v2_module \
--with-http_dav_module \
--with-http_flv_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_xslt_module \
--with-http_stub_status_module \
--with-http_sub_module \
--with-http_random_index_module \
--with-http_degradation_module \
--with-http_secure_link_module \
--with-http_gzip_static_module \
--with-debug \
--with-file-aio \
--with-stream

# 编译安装
make && make install
```

#### Start
```
/usr/local/nginx/sbin/nginx
/usr/local/nginx/sbin/nginx -s reload
/usr/local/nginx/sbin/nginx -s stop
```