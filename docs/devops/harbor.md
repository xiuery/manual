### harbor
镜像仓库

---

#### Links
- [https://goharbor.io/](https://goharbor.io/)

#### Installation
- cp harbor.yml.tmpl harbor.yml

- 配置: hostname/http.port/注释掉https/external_url/harbor_admin_password/database.password
```
# Configuration file of Harbor

# The IP address or hostname to access admin UI and registry service.
# DO NOT use localhost or 127.0.0.1, because Harbor needs to be accessed by external clients.
hostname: 127.0.0.1

# http related config
http:
  # port for http, default is 80. If https enabled, this port will redirect to https port
  port: 8002

# https related config
#https:
  # https port for harbor, default is 443
  #port: 443
  # The path of cert and key files for nginx
  #certificate: /your/certificate/path
  #private_key: /your/private/key/path

# # Uncomment following will enable tls communication between all harbor components
# internal_tls:
#   # set enabled to true means internal tls is enabled
#   enabled: true
#   # put your cert and key files on dir
#   dir: /etc/harbor/tls/internal

# Uncomment external_url if you want to enable external proxy
# And when it enabled the hostname will no longer used
external_url: https://harbor.xiuery.com

# The initial password of Harbor admin
# It only works in first time to install harbor
# Remember Change the admin password from UI after launching Harbor.
harbor_admin_password: password

# Harbor DB configuration
database:
  # The password for the root user of Harbor DB. Change this before any production use.
  password: password
  # The maximum number of connections in the idle connection pool. If it <=0, no idle connections are retained.
  max_idle_conns: 100
  # The maximum number of open connections to the database. If it <= 0, then there is no limit on the number of open connections.
  # Note: the default number of connections is 1024 for postgres of harbor.
  max_open_conns: 900
  # The maximum amount of time a connection may be reused. Expired connections may be closed lazily before reuse. If it <= 0, connections are not closed due to a connection's age.
  # The value is a duration string. A duration string is a possibly signed sequence of decimal numbers, each with optional fraction and a unit suffix, such as "300ms", "-1.5h" or "2h45m". Valid time units are "ns", "us" (or "µs"), "ms", "s", "m", "h".
```

- 启动: ./install.sh

- 修改配置: rm -rf ./common && ./install.sh


#### 基础镜像
```
docker login -u admin -p password localhost:8002

docker images | grep localhost
sudo docker images -a | grep localhost | awk '{print $3}' | xargs sudo docker rmi -f


docker pull debian:11.6
docker pull debian:10.13
docker pull debian:9.13

docker tag debian:11.6 localhost:8002/library/debian:11.6
docker tag debian:10.13 localhost:8002/library/debian:10.13
docker tag debian:9.13 localhost:8002/library/debian:9.13

docker push localhost:8002/library/debian:11.6
docker push localhost:8002/library/debian:10.13
docker push localhost:8002/library/debian:9.13

-----------------------------------------------------------------------------------

docker pull mysql:8.0.32
docker pull mysql:5.7.41

docker tag mysql:8.0.32 localhost:8002/library/mysql:8.0.32
docker tag mysql:5.7.41 localhost:8002/library/mysql:5.7.41

docker push localhost:8002/library/mysql:8.0.32
docker push localhost:8002/library/mysql:5.7.41

-----------------------------------------------------------------------------------

docker pull nginx:1.22
docker pull nginx:1.20
docker pull nginx:1.18
docker pull nginx:1.16
docker pull nginx:1.14
docker pull nginx:1.12
docker pull nginx:1.10

docker tag nginx:1.22 localhost:8002/library/nginx:1.22
docker tag nginx:1.20 localhost:8002/library/nginx:1.20
docker tag nginx:1.18 localhost:8002/library/nginx:1.18
docker tag nginx:1.16 localhost:8002/library/nginx:1.16
docker tag nginx:1.14 localhost:8002/library/nginx:1.14
docker tag nginx:1.12 localhost:8002/library/nginx:1.12
docker tag nginx:1.10 localhost:8002/library/nginx:1.10

docker push localhost:8002/library/nginx:1.22
docker push localhost:8002/library/nginx:1.20
docker push localhost:8002/library/nginx:1.18
docker push localhost:8002/library/nginx:1.16
docker push localhost:8002/library/nginx:1.14
docker push localhost:8002/library/nginx:1.12
docker push localhost:8002/library/nginx:1.10

-----------------------------------------------------------------------------------

docker pull php:8.1-fpm
docker pull php:8.0-fpm
docker pull php:7.4-fpm
docker pull php:5.6-fpm

docker tag php:8.1-fpm localhost:8002/library/php:8.1-fpm
docker tag php:8.0-fpm localhost:8002/library/php:8.0-fpm
docker tag php:7.4-fpm localhost:8002/library/php:7.4-fpm
docker tag php:5.6-fpm localhost:8002/library/php:5.6-fpm

docker push localhost:8002/library/php:8.1-fpm
docker push localhost:8002/library/php:8.0-fpm
docker push localhost:8002/library/php:7.4-fpm
docker push localhost:8002/library/php:5.6-fpm

-----------------------------------------------------------------------------------

docker pull python:3.11
docker pull python:3.10
docker pull python:3.9
docker pull python:3.8
docker pull python:3.7
docker pull python:3.6

docker tag python:3.11 localhost:8002/library/python:3.11
docker tag python:3.10 localhost:8002/library/python:3.10
docker tag python:3.9 localhost:8002/library/python:3.9
docker tag python:3.8 localhost:8002/library/python:3.8
docker tag python:3.7 localhost:8002/library/python:3.7
docker tag python:3.6 localhost:8002/library/python:3.6

docker push localhost:8002/library/python:3.11
docker push localhost:8002/library/python:3.10
docker push localhost:8002/library/python:3.9
docker push localhost:8002/library/python:3.8
docker push localhost:8002/library/python:3.7
docker push localhost:8002/library/python:3.6

-----------------------------------------------------------------------------------

docker pull redis:7.0
docker pull redis:6.2.11
docker pull redis:5.0.14

docker tag redis:7.0 localhost:8002/library/redis:7.0
docker tag redis:6.2.11 localhost:8002/library/redis:6.2.11
docker tag redis:5.0.14 localhost:8002/library/redis:5.0.14

docker push localhost:8002/library/redis:7.0
docker push localhost:8002/library/redis:6.2.11
docker push localhost:8002/library/redis:5.0.14

-----------------------------------------------------------------------------------

docker pull selenium/standalone-chrome:110.0
docker pull selenium/standalone-chrome:109.0
docker pull selenium/standalone-chrome:108.0
docker pull selenium/standalone-chrome:99.0
docker pull selenium/standalone-chrome:98.0

docker tag selenium/standalone-chrome:110.0 localhost:8002/library/selenium/standalone-chrome:110.0
docker tag selenium/standalone-chrome:109.0 localhost:8002/library/selenium/standalone-chrome:109.0
docker tag selenium/standalone-chrome:108.0 localhost:8002/library/selenium/standalone-chrome:108.0
docker tag selenium/standalone-chrome:99.0 localhost:8002/library/selenium/standalone-chrome:99.0
docker tag selenium/standalone-chrome:98.0 localhost:8002/library/selenium/standalone-chrome:98.0

docker push localhost:8002/library/selenium/standalone-chrome:110.0
docker push localhost:8002/library/selenium/standalone-chrome:109.0
docker push localhost:8002/library/selenium/standalone-chrome:108.0
docker push localhost:8002/library/selenium/standalone-chrome:99.0
docker push localhost:8002/library/selenium/standalone-chrome:98.0

-----------------------------------------------------------------------------------

docker pull selenium/standalone-edge:110.0
docker pull selenium/standalone-edge:109.0
docker pull selenium/standalone-edge:108.0
docker pull selenium/standalone-edge:99.0
docker pull selenium/standalone-edge:98.0

docker tag selenium/standalone-edge:110.0 localhost:8002/library/selenium/standalone-edge:110.0
docker tag selenium/standalone-edge:109.0 localhost:8002/library/selenium/standalone-edge:109.0
docker tag selenium/standalone-edge:108.0 localhost:8002/library/selenium/standalone-edge:108.0
docker tag selenium/standalone-edge:99.0 localhost:8002/library/selenium/standalone-edge:99.0
docker tag selenium/standalone-edge:98.0 localhost:8002/library/selenium/standalone-edge:98.0

docker push localhost:8002/library/selenium/standalone-edge:110.0
docker push localhost:8002/library/selenium/standalone-edge:109.0
docker push localhost:8002/library/selenium/standalone-edge:108.0
docker push localhost:8002/library/selenium/standalone-edge:99.0
docker push localhost:8002/library/selenium/standalone-edge:98.0

-----------------------------------------------------------------------------------

docker pull hhyo/archery:v1.9.1
docker pull hanchuanchuan/goinception:v1.3.0
docker pull keking/kkfileview:4.1.0

docker tag hhyo/archery:v1.9.1 localhost:8002/app/archery:v1.9.1
docker tag hanchuanchuan/goinception:v1.3.0 localhost:8002/app/goinception:v1.3.0
docker tag keking/kkfileview:4.1.0 localhost:8002/app/kkfileview:4.1.0

docker push localhost:8002/app/archery:v1.9.1
docker push localhost:8002/app/goinception:v1.3.0
docker push localhost:8002/app/kkfileview:4.1.0
```