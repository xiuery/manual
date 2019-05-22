### [RocketMQ](http://rocketmq.apache.org/docs/quick-start/)
消息队列

---

#### Installation
```
# 环境要求：Maven 3.2.x并且服务器要有权限
# 下载解压 /usr/local/rocketmq-all-4.5.0/
# 编译
mvn -Prelease-all -DskipTests clean install -U
```

#### Start
```
cd distribution/target/apache-rocketmq
nohup sh bin/mqnamesrv &
nohup sh bin/mqbroker -n localhost:9876 &
sh bin/mqshutdown broker
sh bin/mqshutdown namesrv

# 运维命令
bin/mqadmin clusterList -n 127.0.0.1:9876                       # 查看集群情况
bin/mqadmin brokerStatus -n 127.0.0.1:9876 -b 10.0.0.41:10911   # 查看broker状态
bin/mqadmin topicList -n 127.0.0.1:9876                         # 查看topic列表
bin/mqadmin topicStatus -n 127.0.0.1:9876 -t TopicTest          # 查看topic状态
bin/mqadmin topicRoute -n 127.0.0.1:9876 -t TopicTest           # 查看topic路由
```

#### 开机启动
```
echo 'nohup sh /usr/local/rocketmq-all-4.5.0/distribution/target/apache-rocketmq/bin/mqnamesrv &' >> /etc/rc.local
echo 'nohup sh /usr/local/rocketmq-all-4.5.0/distribution/target/apache-rocketmq/bin/mqbroker -n localhost:9876 &' >> /etc/rc.local
```

#### 日志
```
tail -f ~/logs/rocketmqlogs/namesrv.log
tail -f ~/logs/rocketmqlogs/broker.log
```

#### 测试
```
export NAMESRV_ADDR=localhost:9876
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
```

#### web管理
```
# https://github.com/apache/rocketmq-externals/tree/master/rocketmq-console
mvn clean package -Dmaven.test.skip=true
nohup java -jar target/rocketmq-console-ng-1.0.1.jar &
```

#### 启动异常
```
# 找不到JAVA_HOME、Cannot allocate memory找到指定文件用修改以下内容
# /usr/local/rocketmq-all-4.5.0/distribution/target/apache-rocketmq
# runserver.sh、runbroker.sh
JAVA_HOME=/usr/local/jdk1.8.0_211
JAVA_OPT="${JAVA_OPT} -server -Xms512M -Xmx512M -Xmn256M -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"
```