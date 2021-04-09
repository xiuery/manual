### [Jenkins](https://www.jenkins.io/zh/doc/)
automate all sorts of tasks related to building, testing, and delivering or deploying

---

#### [Installation](https://www.jenkins.io/doc/book/installing/linux/)
```
# 先安装Java环境

# 添加repository安装
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum upgrade
sudo yum install jenkins

# 配置Java路径
vi /etc/init.d/jenkins
> candidates="
  /usr/local/jdk1.8.0_281/bin/java
  /etc/alternatives/java
  /usr/lib/jvm/java-1.8.0/bin/java
  /usr/lib/jvm/jre-1.8.0/bin/java
  /usr/lib/jvm/java-11.0/bin/java
  /usr/lib/jvm/jre-11.0/bin/java
  /usr/lib/jvm/java-11-openjdk-amd64
  /usr/bin/java
"
# 配置端口
vi /etc/sysconfig/jenkins
> JENKINS_PORT="8080"

# Reload
sudo systemctl daemon-reload

# 打开防火墙
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --reload

# COMMAND: start, stop, restart, force-reload and status
sudo systemctl start jenkins
sudo systemctl status jenkins
sudo systemctl stop jenkins
sudo systemctl restart jenkins
```

#### Jenkinsfile

- 完成时动作
```
post {
    always {
        echo 'This will always run'
    }
    success {
        echo 'This will run only if successful'
    }
    failure {
        echo 'This will run only if failed'
    }
    unstable {
        echo 'This will run only if the run was marked as unstable'
    }
    changed {
        echo 'This will run only if the state of the Pipeline has changed'
        echo 'For example, if the Pipeline was previously failing but is now successful'
    }
}
```