### Gitlab
open DevOps platform

---

#### [Installation](https://about.gitlab.com/install)
```
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


```