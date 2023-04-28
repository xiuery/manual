### Gitlab
open DevOps platform

---

#### Links
- [Installation](https://about.gitlab.com/install)
- [Install Docker](https://docs.gitlab.com/ee/install/docker.html)

#### Installation
```
# 卸载
# 停止服务
gitlab-ctl stop
# 卸载
rpm -e gitlab-ee
# 查看gitlab进程并kill
ps -ef | grep gitlab 或者 ps aux | grep gitlab
# 删掉配置
find / -name gitlab | xargs rm -rf

# 安装依赖
sudo yum install -y curl policycoreutils-python openssh-server perl

# 启用sshd
sudo systemctl enable sshd
sudo systemctl start sshd

# 打开http/https端口
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo systemctl reload firewalld

# 可选：Postfix 
sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix

# 添加repository安装
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
EXTERNAL_URL="http://10.0.0.102" yum install -y gitlab-ee

# 配置目录
vi /etc/gitlab/gitlab.rb

# COMMAND
sudo gitlab-ctl reconfigure
sudo gitlab-ctl restart|status|stop|start

# 启动报错
# 1.warning: redis: unable to open supervise/ok: file does not exist
systemctl restart gitlab-runsvdir
gitlab-ctl reconfigure
# 2.地址访问502报错：机器配置过低，关闭其他服务重启或者分配资源重装
```

#### Docker安装
- docker-compose.yml
```
version: '3.6'
services:
  gitlab:
    image: localhost:8002/devops/gitlab-ce:15.11.0-ce.0
    container_name: gitlab
    hostname: gitlab
    restart: always
    environment:
      TZ: Asia/Shanghai
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://gitlab.xiuery.com:8929'
        gitlab_rails['gitlab_shell_ssh_port'] = 2223
    ports:
      - '8929:8929'
      - '2223:22'
    volumes:
      - './config:/etc/gitlab'
      - './logs:/var/log/gitlab'
      - '/data/gitlab:/var/opt/gitlab'
    shm_size: '256m'
```

- 启动
```
docker-compose -f docker-compose.yml up -d
```

- root密码
```
cat ./config/initial_root_password
```
