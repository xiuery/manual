### acme.sh
An ACME Shell script

---

#### Links
- [acme.sh](https://github.com/acmesh-official/acme.sh)
- [acme.sh](https://github.com/acmesh-official/acme.sh/wiki/%E8%AF%B4%E6%98%8E)
- [dns api: 各域名服务商API](https://github.com/acmesh-official/acme.sh/wiki/dnsapi)
- [Run acme.sh in docker](https://github.com/acmesh-official/acme.sh/wiki/Run-acme.sh-in-docker#3-run-acmesh-as-a-docker-daemon)
- [deploy to docker containers](https://github.com/acmesh-official/acme.sh/wiki/deploy-to-docker-containers)

#### 安装
- Install from web
```
curl https://get.acme.sh | sh -s email=xiuery.com@gmail.com

or

wget -O -  https://get.acme.sh | sh -s email=xiuery.com@gmail.com
```

- Install from GitHu
```
curl https://raw.githubusercontent.com/acmesh-official/acme.sh/master/acme.sh | sh -s -- --install-online -m  my@example.com

or

wget -O -  https://raw.githubusercontent.com/acmesh-official/acme.sh/master/acme.sh | sh -s -- --install-online -m  my@example.com
```

- Advanced installation
```
git clone https://github.com/acmesh-official/acme.sh.git
cd acme.sh
./acme.sh --install  \
--home ~/myacme \
--config-home ~/myacme/data \
--cert-home  ~/mycerts \
--accountemail  "my@example.com" \
--accountkey  ~/myaccount.key \
--accountconf ~/myaccount.conf \
--useragent  "this is my client."

# 参数说明
--home is a customized dir to install acme.sh in. By default, it installs into ~/.acme.sh
--config-home is a writable folder, acme.sh will write all the files(including cert/keys, configs) there. By default, it's in --home
--cert-home is a customized dir to save the certs you issue. By default, it's saved in --config-home.
--accountemail   is the email used to register an account to Let's Encrypt, you will receive a renewal notice email here.
--accountkey is the file saving your account private key. By default, it's saved in --config-home.
--useragent is the user-agent header value used to send to Let's Encrypt.
--nocron install acme.sh without cronjob
```

#### 签发SSL证书
- HTTP验证: 域名已经绑定到了所在服务器上，且可以通过公网进行访问
```
# Webroot mode: 服务器在运行着的，网站域名为 example.com，根目录为 /home/wwwroot/example.com
acme.sh  --issue  -d example.com  -w /home/wwwroot/example.com

# Apache/Nginx mode: 
# 1.Apache或者Nginx 服务器，可以自动寻找配置文件来进行签发
acme.sh  --issue  -d example.com  --apache  # Apache
acme.sh  --issue  -d example.com  --nginx   # Nginx
# 2.找不到配置文件的话可以自行配置
acme.sh  --issue  -d example.com  --nginx /etc/nginx/nginx.conf
acme.sh  --issue  -d example.com  --nginx /etc/nginx/conf.d/example.com.conf

# Standalone mode: acme.sh 会自己建立一个服务器来完成签发
# 1.http模式，80端口
acme.sh  --issue  -d example.com  --standalone
# 2.反代之类的不是80端口，则可以手动指定
acme.sh  --issue  -d example.com  --standalone --httpport 88
# 3.支持tls模式，不是443端口的话也可以自行指定
acme.sh  --issue  -d example.com  --alpn
acme.sh  --issue  -d example.com  --alpn --tlsport 8443  # 自行指定tls端口
```

- DNS验证: 需要任何服务器，不需要任何公网ip，只需要dns的解析记录
```
# DNS API mode: 利用域名服务商提供的API就可以自动帮你添加TXT记录完成验证和证书签发
# 1.CloudFlare or Zone.DNS
# CloudFlare
export CF_Token="xxxxxxxxxxxxx"
export CF_Account_ID="xxxxxxxxxxxxx"
# Zone.DNS
export CF_Token="xxxxxxxxxxxxx"
export CF_Account_ID="xxxxxxxxxxxxx"
export CF_Zone_ID="xxxxxxxxxxxxx"
# 生成
acme.sh --issue --dns dns_cf -d example.com

# 2.国内一般用的是DNSPod，也提供了API
export DP_Id="1234"
export DP_Key="sADDsdasdgdsf"
acme.sh --issue --dns dns_dp -d example.com -d www.example.com

# 3.Aliyun
export Ali_Key="xxxxxxxxxxxxx"
export Ali_Secret="xxxxxxxxxxxxx"
acme.sh --issue --dns dns_ali -d example.com -d www.example.com

# DNS manual mode: 域名服务商没有提供API的情况，需要自己在域名配置一个TXT记录，且不能自动续期，每次都需要重新配置
acme.sh  --issue  -d example.com  --dns  -d www.example.com

# DNS alias mode: 域名服务商没有提供API
```

- 多域名配置
```
acme.sh  --issue  -d example.com  -w /home/wwwroot/example.com -d www.example.com
acme.sh  --issue  -d example.com  --standalone  -d www.example.com
acme.sh  --issue  -d example.com  --dns  -d www.example.com
```

- 泛域名配置: 只适用于DNS验证
```
acme.sh  --issue  -d example.com  -d '*.example.com'  --dns dns_cf
```

#### 安装证书
- Nginx
```
acme.sh --installcert -d example.com \
--key-file /path/to/keyfile/in/nginx/key.pem  \
--fullchain-file /path/to/fullchain/nginx/cert.pem \
--reloadcmd "service nginx force-reload"
```

- Apache
```
acme.sh --install-cert -d example.com \
--cert-file /path/to/certfile/in/apache/cert.pem  \
--key-file /path/to/keyfile/in/apache/key.pem  \
--fullchain-file /path/to/fullchain/certfile/apache/fullchain.pem \
--reloadcmd "service apache2 force-reload"
```

##### 配置
- Nginx
```
server {
    listen       443 ssl;
    server_name  xiuery.com;
    access_log  /var/log/nginx/https.log  main;

    ssl_certificate      full_chain.pem;
    ssl_certificate_key  private.key;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;

    ...
}
```

#### 更新证书
```
crontab -l
# 43 0 * * * "/root/.acme.sh"/acme.sh --cron --home "/root/.acme.sh" > /dev/null

# 强制更新
acme.sh --renew -d example.com --force
acme.sh --renew -d example.com --force --ecc  # 如果用的是ECC证书
```

#### 停止更新证书
```
# 证书列表
acme.sh --list

# 停止更新，执行后移除证书文件
acme.sh --remove -d example.com [--ecc]
```

#### 升级
```
acme.sh --upgrade    # 手动升级
acme.sh --upgrade --auto-upgrade    # 自动升级
acme.sh --upgrade --auto-upgrade 0  # 停止自动升级
```

#### docker
- docker images
```
docker pull neilpang/acme.sh:3.0.5
docker tag neilpang/acme.sh:3.0.5 localhost:8002/devops/acme.sh:3.0.5
docker push localhost:8002/devops/acme.sh:3.0.5
```

- .env
```
Ali_Key=xxxxxxxxxxxxx
Ali_Secret=xxxxxxxxxxxxx
```

- Run as docker daemon
```
version: '3.4'
services:
  acme.sh:
    image: localhost:8002/devops/acme.sh:3.0.5
    container_name: acme.sh
    hostname: acme.sh
    command: daemon
    restart: always
    env_file:
      - .env
```

- command
```
# 启动
docker-compose -f docker-compose.yml up -d

# 进入容器
docker exec -ti acme.sh sh

# 注册账户
acme.sh --register-account -m xiuery.com@gmail.com

# aliyun为例：用户 -> AccessKey管理 -> 子用户AccessKey -> 添加账户并授权AliyunDNSFullAccess

# 泛域名
acme.sh --issue --dns dns_ali -d xiuery.com -d *.xiuery.com

# debug
acme.sh --issue --dns dns_ali -d xiuery.com -d '*.xiuery.com' --debug
acme.sh --issue --dns dns_ali -d xiuery.com -d '*.xiuery.com' --debug 2
```
