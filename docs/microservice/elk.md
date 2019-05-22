### ELK
ElasticSearch + Logstash + Kiabana + Filebeat

#### LINKS
- [docker@elastic](https://www.docker.elastic.co/)

#### Elasticsearch
+ 关于集群配置
    * discovery.zen.ping.unicast.hosts: Elasticsearch默认使用Zen Discovery来做节点发现机制
    * discovery.zen.minimum_master_nodes: 该参数表示集群中可工作的具有Master节点资格的最小数量，默认值是1
    * discovery.zen.ping_timeout: 表示节点在发现过程中的等待时间
+ 关于集群节点
    * 节点类型包括：候选Master节点、数据节点和Client节点
    * 尽量将候选Master节点和Data节点分离开，通常Data节点负载较重，需要考虑单独部署
+ 关于内存
    * Elasticsearch默认设置的内存是1GB，通过指定ES_HEAP_SIZE环境变量，可以修改其堆内存大小
+ 关于硬盘空间
    * Elasticsearch默认将数据存储在/var/lib/elasticsearch路径下
    * 通过path.data配置项来进行设置，比如: path.data: /data1,/var/lib/elasticsearch,/data
    * 需要注意的是: 同一分片下的数据只能写入到一个路径下，因此还是需要合理的规划和监控硬盘的使用
+ 关于Index的划分和分片的个数
    * 根据数据量来做权衡，Index可以按时间划分
    * 比如每月一个或者每天一个，在Logstash输出时进行配置，shard的数量也需要做好控制
+ 关于监控
    * head和marvel两个监控插件，head免费，功能相对有限，marvel现在需要收费了
		* 不要在数据节点开启监控插件

### nginx日志格式
- 配置格式
```
# 字符格式
log_format main '$remote_addr "$time_iso8601" "$request" $status $body_bytes_sent'
		'"$http_user_agent" "$http_referer" "$http_x_forwarded_for" "$request_time"'
		'"$upstream_response_time" "$http_cookie" "$http_Authorization" "$http_token"';

# json格式
log_format main '{'
		'"remote_addr": "$remote_addr",'
		'"remote_user": "$remote_user",'
		'"time_local": "$time_local",'
		'"http_user_agent": "$http_user_agent",'
		'"http_x_forwarded_for": "$http_x_forwarded_for",'
		'"http_cookie": "$http_cookie",'
		'"http_Authorization": "$http_Authorization",'
		'"http_token": "$http_token",'
		'"http_referer": "$http_referer",'
		'"upstream_addr": "$upstream_addr",'
		'"ssl_protocol": "$ssl_protocol",'
		'"ssl_cipher": "$ssl_cipher",'
		'"http_host": "$http_host",'
		'"request": "$request",'
		'"status": "$status",'
		'"upstream_status": "$upstream_status",'
		'"body_bytes_sent": "$body_bytes_sent",'
		'"request_time": "$request_time",'
		'"upstream_response_time": "$upstream_response_time"'
		'}';
```

- 完整参数格式
```
$remote_addr             客户端地址                                    211.28.65.253
$remote_user             客户端用户名称                                --
$time_local              访问时间和时区                                18/Jul/2012:17:00:01 +0800
$request                 请求的URI和HTTP协议                           "GET /article-10000.html HTTP/1.1"
$http_host               请求地址，即浏览器中你输入的地址（IP或域名）     www.wang.com 192.168.100.100
$status                  HTTP请求状态                                  200
$upstream_status         upstream状态                                  200
$body_bytes_sent         发送给客户端文件内容大小                        1547
$http_referer            url跳转来源                                   https://www.baidu.com/
$http_user_agent         用户终端浏览器等信息                           "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.0; SV1; GTB7.0; .NET4.0C;
$ssl_protocol            SSL协议版本                                   TLSv1
$ssl_cipher              交换数据中的算法                               RC4-SHA
$upstream_addr           后台upstream的地址，即真正提供服务的主机地址    10.10.10.100:80
$request_time            整个请求的总时间                               0.205
$upstream_response_time  请求过程中，upstream响应时间                   0.002
```

### Elasticsearch
- 拉取镜像
```
sudo docker pull docker.elastic.co/elasticsearch/elasticsearch:6.4.0
```

- 快速启动
```
sudo docker run -p 9200:9200 -p 9300:9300 \
	-e "discovery.type=single-node" \
	docker.elastic.co/elasticsearch/elasticsearch:6.4.0
```

- PRODUCTION MODE
```
# 查看vm.max_map_count
grep vm.max_map_count /etc/sysctl.conf
#
sysctl -w vm.max_map_count=262144
```

- config
```
https://www.elastic.co/guide/en/elasticsearch/reference/6.4/settings.html
```

- docker-compose.yml
```
version: '2'
services:
  elasticsearch-node1:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.4.0
    container_name: elasticsearch1
    environment:
      - cluster.name=docker-cluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data-node1:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - esnet
  elasticsearch-node2:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.4.0
    container_name: elasticsearch2
    environment:
      - cluster.name=docker-cluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - "discovery.zen.ping.unicast.hosts=elasticsearch"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data-node2:/usr/share/elasticsearch/data
    networks:
      - esnet

volumes:
  data-node1:
    driver: local
  data-node2:
    driver: local

networks:
  esnet:
```

### kibana
- 拉取镜像
```
sudo docker pull docker.elastic.co/kibana/kibana:6.4.0
```

- docker-compose.yml
```
version: '2'
services:
  kibana:
    image: docker.elastic.co/kibana/kibana:6.4.0
    ports:
      - 5601:5601
    environment:
      SERVER_NAME: kibana
      ELASTICSEARCH_URL: http://10.0.0.52:9200
```
- config
```
https://www.elastic.co/guide/en/kibana/6.4/settings.html
```

### logstash
- 拉取镜像
```
sudo docker pull docker.elastic.co/logstash/logstash:6.4.0
```

- 启动容器
```
sudo docker run --rm -it \
	-v /home/projects/elk/logstash/pipeline/:/usr/share/logstash/pipeline/ \
	docker.elastic.co/logstash/logstash:6.4.0 \
	/bin/bash

sudo docker run --rm -it \
	-v /home/projects/elk/logstash/pipeline/:/usr/share/logstash/pipeline \
	docker.elastic.co/logstash/logstash:6.4.0

sudo docker run --rm -it \
	-v /home/projects/elk/logstash/config:/usr/share/logstash/config \
	docker.elastic.co/logstash/logstash:6.4.0

sudo docker run --rm -it \
	-v /home/projects/elk/logstash/config/logstash.yml:/usr/share/logstash/config/logstash.yml \
	docker.elastic.co/logstash/logstash:6.4.0
```

- logstash.conf
```
# Sample Logstash configuration for creating a simple
# Beats -> Logstash -> Elasticsearch pipeline.

input {
	stdin { }
}

output {
  elasticsearch {
    hosts => ["http://10.0.0.52:9200"]
    index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
    #user => "elastic"
    #password => "changeme"
  }
}

```

- logstash.yml
```
http.host: "0.0.0.0"
xpack.monitoring.elasticsearch.url: http://10.0.0.52:9200
```

- 容器内部启动服务
```
logstash -f /usr/share/logstash/config/logstash-sample.conf
```

### filebeat

- 基本使用
```
# 列出模块
filebeat modules list
# 使模块生效
filebeat modules enable nginx
# 启动filebeat
filebeat setup
#
filebeat run
```

- filebeat.yml说明(完整配置看filebeat.reference.yml)
```
# 默认值 log ，表示一个日志读取源：这个源是收集 Nginx 的访问日志
type : log

# 该配置是否生效，如果设置为 false 将不会收集该配置的日志
enabled: true

# 要抓取的日志路径，写绝对路径
paths: /var/log/nginx/*.log

# fields 表示自定义字段，在下面缩进两格处写要自己添加的字段
# 如： alilogtype: usercenter_serverlog  表示在输出的每条日志中加入该字段，
# key：alilogtype, value：usercenter_serverlog 用于标识该日志源的类别，在传输到下一层 logstash 时可以根据该字段分类处理
# key：serverip, value: ${serverip} 这个值是读取的系统环境变量，如果系统中没有定义这个环境变量，那么启动 filebeat 的时候会报错，找到这个值
fields:
	alilogtype: usercenter_serverlog
	serverip: ${serverip}

## 设置系统环境变量，创建文件  /etc/profile.d/serverip.sh  加入内容：
export serverip=`ifconfig eth0 | grep 'inet addr' | awk '{print $2}' | cut -d':' -f2`
## 这里拿的是本机 IP

## 多行合并参数，正则表达式
multiline.pattern: '^\['
## true 或 false；默认是false，匹配pattern的行合并到上一行；true，不匹配pattern的行合并到上一行
multiline.negate: true
## after 或 before，合并到上一行的末尾或开头
multiline.match: after

## 属性可以配置只收集error级别和warn级别的日志,如果有配置多行收集,一定要将这个配置放在多行的后面
include_lines: ['ERROR','WARN']
## 属性配置不收集DEBUG级别的日志,如果配置多行 这个配置也要放在多行的后面
exclude_lines: ['DEBUG']
```
