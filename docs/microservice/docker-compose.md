docker-compose
容器集合

---

#### Links

#### 

#### docker-compose命令
- 安装
```
# 安装: 注意版本
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

- 命令
```
# 启动
docker-compose -f docker-compose.yml up -d

# 列出项目中目前的所有容器
docker-compose ps
docker-compose ps -a

# 重启项目中的服务
docker-compose restart

# 暂停一个服务容器
docker-compose pause

# 恢复处于暂停状态中的服务
docker-compose unpause

# 停止正在运行的容器
docker-compose stop

# 删除所有（停止状态的）服务容器
docker-compose rm
–f, –force，强制直接删除，包括非停止状态的容器
-v，删除容器所挂载的数据卷

# 启动已经存在的服务容器
docker-compose start

# 停止和删除容器、网络、卷、镜像
docker-compose down
–rmi type，删除镜像，类型必须是：all，删除compose文件中定义的所有镜像；local，删除镜像名为空的镜像
-v, –volumes，删除已经在compose文件中定义的和匿名的附在容器上的数据卷
–remove-orphans，删除服务中没有在compose中定义的容器

# 验证并查看compose文件配置
docker-compose config
–resolve-image-digests 将镜像标签标记为摘要
-q, –quiet 只验证配置，不输出。 当配置正确时，不输出任何内容，当文件配置错误，输出错误信息
–services 打印服务名，一行一个
–volumes 打印数据卷名，一行一个

# 查看服务容器的输出
docker-compose logs

# 在指定服务上执行一个命令
docker-compose run
docker-compose run ubuntu ping www.baidu.com

# 构建（重新构建）项目中的服务容器
docker-compose build
–compress 通过gzip压缩构建上下环境
–force-rm 删除构建过程中的临时容器
–no-cache 构建镜像过程中不使用缓存
–pull 始终尝试通过拉取操作来获取更新版本的镜像
-m, –memory MEM为构建的容器设置内存大小
–build-arg key=val为服务设置build-time变量
```

#### [Selenium](https://hub.docker.com/u/selenium)
- docker run

```
# standalone与node的区别: standalone独立运行; node集群运行

# 运行standalone-chrome
docker run -d --name standalone-chrome --restart=always -p 4444:4444 -p 5900:5900 -p 7900:7900 --shm-size="2g" docker.xiuery.com/library/selenium/standalone-chrome:110.0
or 
docker run -d --name standalone-chrome --restart=always  -p 4444:4444 -p 7900:7900 --shm-size="2g" docker.xiuery.com/library/selenium/standalone-chrome:110.0

# web访问:
http://127.0.0.1:4444
http://127.0.0.1:7900

# 首次登录，修改vnc密码
touch /home/seluser/.vnc/passwd
x11vnc -storepasswd password /home/seluser/.vnc/passwd
```

- docker-compose
```
version: '3'

services:
  standalone-chrome:
    image: harbor.xiuery.com/library/selenium/standalone-chrome:110.0
    container_name: standalone-chrome
    shm_size: 2g
    restart: always
    ports:
      - "4444:4444"
      #- "5900:5900"
      - "7900:7900"
```