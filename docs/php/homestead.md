### Homestead
laravel集成环境（vagrant2.2.9 + laravel/homestead10.0.0）

---

#### [Vagrant](https://www.vagrantup.com/docs)
https://www.vagrantup.com/downloads

#### [Box](https://app.vagrantup.com/boxes/search)
- 下载离线版：指定的版本 + /providers/virtualbox.box
- https://app.vagrantup.com/laravel/boxes/homestead/versions/10.0.0/providers/virtualbox.box

#### 安装 Vagrant Box
```
# 在线安装
vagrant box add laravel/homestead

# 离线安装
vagrant box add laravel/homestead ~/Desktop/xiuery/virtualbox.box
vagrant box add --clean laravel/homestead ~/Desktop/xiuery/virtualbox.box

# 修改版本
cd ~/.vagrant.d/boxes/laravel-VAGRANTSLASH-homestead
mv 0 10.0.0

# 查看box列表
vagrant box list

# 更新
vagrant box update

# 移除box
vagrant box remove laravel/homestead
```

#### 安装 Homestead
```
cd ~/Desktop/xiuery/
git clone https://github.com/laravel/homestead.git Homestead
cd Homestead
git checkout release
# Mac、Linux
bash init.sh
# Windows
init.bat
```

#### 配置 Homestead（Homestead.yaml）
- 提供器：virtualbox, vmware_fusion, vmware_workstation, parallels 以及 hyperv
```
provider: virtualbox
```

- 共享文件夹：所有与 Homestead 环境共享的文件夹，这些文件夹中的文件如果发生变更，它们会保持本地机器与 Homestead 环境之间同步
```
folders:
    - map: ~/Desktop/xiuery/xs-auth
      to: /home/vagrant/xs-auth
      # type: "nfs"

    - map: ~/Desktop/xiuery/xs-app
      to: /home/vagrant/xs-app
```

- Nginx 站点：映射一个” 域名” 到一个文件夹
```
sites:
    - map: xs-auth.test
      to: /home/vagrant/xs-auth/public
      #  多版本php
      php: "7.1"
      # apache，apigility，expressive，laravel（默认），proxy，silverstripe，statamic，symfony2，symfony4，和 zf
      # type: "laravel"
      # Laravel 提供了一种 定时计划作业 的方式
      # 只需每分钟运行一次 schedule:run Artisan 
      # 检查在你在 App\Console\Kernel 类中定义的计划
      schedule: true
      # 通过 params 站点指令添加额外的 Nginx fastcgi_param 值到你的站点
      params:
        - key: FOO
          value: BAR

    - map: xs-app.test
      to: /home/vagrant/xs-app/public
```
```
# 更改需要reload
vagrant reload --provision

# reload出错执行
vagrant destroy && vagrant up
```

- 数据库：
```
host: 127.0.0.1
port: 33060 （MySQL）| 54320 （PostgreSQL）
username: homestead
password: secret
```

- 环境变量
```
variables:
    - key: APP_ENV
      value: DEV
```

- 端口
```
# 默认端口
SSH：2222 -> 转发到 22
ngrok UI：4040 -> 转发到 4040
HTTP：8000 -> 转发到 80
HTTPS：44300 -> 转发到 443
MySQL：33060 -> 转发到 3306
PostgreSQL：54320 -> 转发到 5432
MongoDB：27017 -> 转发到 27017
Mailhog：8025 -> 转发到 8025
Minio：9600 -> 转发到 9600
```
```
# 指定端口转发
ports:
    - send: 50000
      to: 5000
    - send: 7777
      to: 777
      protocol: udp
```

#### 启动 vagrant
```
# 切换到Homestead目录
cd ~/Desktop/Homestead
# 启动
vagrant up

# reload
vagrant reload --provision

# reload出错执行
vagrant destroy && vagrant up

# 销毁
vagrant destroy --force

# ssh
vagrant ssh
```

#### 全局访问 Homestead
- Mac、Linux：脚本写入 ~/.bash_profile
```
function homestead() {
    ( cd ~/Desktop/Homestead && vagrant $* )
}
```

- Windows：脚本写入环境变量
```
@echo off

set cwd=%cd%
set homesteadVagrant=C:\Homestead

cd /d %homesteadVagrant% && vagrant %*
cd /d %cwd%

set cwd=
set homesteadVagrant=
```
