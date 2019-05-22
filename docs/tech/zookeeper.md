### [ZooKeeper](https://zookeeper.apache.org/doc/r3.4.14/zookeeperStarted.html)
注册中心

---

#### Installation
```
# 下载解压到/usr/local/zookeeper
# 修改配置conf/zoo.cfg
```

#### Start
```
bin/zkServer.sh start
bin/zkServer.sh {start|start-foreground|stop|restart|status|upgrade|print-cmd}
```

#### Client
```
bin/zkCli.sh -server 127.0.0.1:2181
```