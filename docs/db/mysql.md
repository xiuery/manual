### Mysql
mysql的常用语法

---

#### 0.常用
- 修改密码
```
mysql>update user set password=password('root') where user='root' and host='localhost';
```

- 清空表
```
#只清空数据
mysql>delete from xx
#清空数据，并且重置auto_increment
mysql>truncate table xx
```

#### 1.数据导出
- 所有库
```
$ /usr/bin/mysqldump -u root -proot\!123\$ --all-databases > databases.sql
```

- 除指定库中的指定表外的所有库所有表
```
$ /usr/bin/mysqldump -u root -proot\!123\$ --all-databases --ignore-table=xs.app_logs > databases.sql
```

- 指定库中所有表
```
$ /usr/bin/mysqldump -u root -proot\!123\$ xs > xs-datas.sql
```

- 单张表
```
$ /usr/bin/mysqldump -u root -proot\!123\$ xs app_logs > xs-app_logs-datas.sql
```

- 除指定表之外，可指定多张表
```
$ /usr/bin/mysqldump -u root -proot\!123\$ xs --ignore-table=xs.app_logs --ignore-table=xs.app_user > xs-ignore-app_logs-app_user-datas.sql
```

- 单张表+条件
```
$ /usr/bin/mysqldump -u root -proot\!123\$ xs app_logs --where='ID<49000000' > xs-app_logs-datas-1.sql
$ /usr/bin/mysqldump -u root -proot\!123\$ xs app_logs --where='ID>48888889' > xs-app_logs-datas-2.sql
```

- 导出结构不导出数据
```
$ /usr/bin/mysqldump -u root -proot\!123\$　--opt　-d　数据库名　>　xxx.sql　
```

- 导出数据不导出结构
```
$ /usr/bin/mysqldump -u root -proot\!123\$　-t　数据库名　>　xxx.sql
```

- 其他选项
```
--all--database/-A
--add-drop--database
--add-drop-tables
--add-locking           #用LOCK TABLES和UNLOCK TABLES语句引用每个表转储
--allow-keywords        #允许创建关键字列名
---comments[={0|1}]
--compact	            #该选项禁用注释并启用--skip-add-drop-tables、--no-set-names、--skip-disable-keys和--skip-add-locking选项
```

#### 2.特殊查询
- 字符匹配截取
```
substring_index(字段名, 匹配字符, 从第几个开始匹配)    #返回匹配成功位置
substring(字段名, 开始位置, 长度)                    #返回截取后字段
# example：
trim(substring(title,length(substring_index(title,'-',3))+2))
```

- 条件匹配
```
where title REGEXP '-'
```

- 合并多个查询结果
```
union all
```

- 复制表结构
```
create table `xs`.`app_log_1` like `xs`.`app_log`
```

- 复制表结构+数据
```
create table `xs`.`app_log_1` as select * from `xs`.`app_log`
```

- 查询插入记录
```
mysql>insert into `xs`.`app_log`
select
`id` as `id`,
`user_name` as `username`
`user_id` as `user_id`
`create_time` as `create_time`
`update_time` as `update_time`
`delete_time` as `delete_time`
from `xs`.`app_activity`
where id > 1000000
```
- 查询同一字段值存在多个
```
select `id`,`name`,count(`name`) from `xs`.`app_user` group by `name` having count(`name`) > 1
```

#### 3.数据库脚本
```
/usr/bin/mysql -uroot -proot\!123\$ << EOF
insert into `xs`.`app_log`
select
`id` as `id`,
`user_name` as `username`
`user_id` as `user_id`
`create_time` as `create_time`
`update_time` as `update_time`
`delete_time` as `delete_time`
from `xs`.`app_activity`
where id > 1000000
EOF
```

#### 4.数据库备份脚本
```
#!/bin/bash
DB_USER=root
DB_PWD=root!123$

cd /backup

#echo "------------- start backup web -------------"
#WEBBAK=webbak_$(date +%y%m%d).tar.xz
#不备份指定目录文件
#tar --exclude=/home/wwwroot/app/cache/* -zcvf $WEBBAK  /home/wwwroot/app
#echo "------------- end backup web -------------"

echo "------------- start backup database -------------"

echo "##### xs ignore app_logs"
XSIGNORELOGSBAK=xs-ignore-app_logs.sql
XSIGNORELOGSBAKTAR=xs-ignore-app_logs-$(date +%y%m%d).tar.xz
/usr/bin/mysqldump -u$DB_USER -p$DB_USER --ignore-table=xs.app_logs > $XSIGNORELOGSBAK
tar zcvf $XSIGNORELOGSBAKTAR $XSIGNORELOGSBAK
```

#### 5.添加用户并授权
```
# 创建用户 - 添加权限
create user xiuery@'%' identified by 'xiuery';
grant select,update,delete on *.* to xiuery;

# or 创建用户并添加权限
grant select,update,delete on *.* to 'xiuery'@'%' identified by 'xiuery';

# 使修改立即生效
flush privileges;
```

#### 6.授权远程连接
```
# 添加新用户
grant select,update,delete on *.* to 'xiuery'@'%' identified by 'xiuery';

# 修改my.cnf
vi /etc/my.cnf
skip-networking             # 注释掉
bind-address = 0.0.0.0      # 需要连接的ip地址

# 重启mysql

# 如果还是无法远程连接，就要修改服务器的防火墙了
# ×××××××× 注意：这是很不安全的 ×××××××××
# 打开防火墙配置文件
vi /etc/sysconfig/iptables
# 开放3306
-A INPUT -p tcp -m tcp --dport 3306 -j ACCEPT
```

#### 7.错误处理
- ONLY_FULL_GROUP_BY报错
```
#在sql_mode中删除ONLY_FULL_GROUP_BY
$ vi /etc/my.cnf
sql_mode = 'STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'

or

mysql>set global sql_mode = 'STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
```

- 支持federated引擎
```
#在配置文件中添加federated
$ echo 'federated' >> /etc/my.cnf
```

- 数据库表crashed修复
```
#检查指定表
$ /usr/bin/mysqlcheck -u root -proot\!123\$ -c xs app_logs
#修复指定表
$ /usr/bin/mysqlcheck -u root -proot\!123\$ -r xs app_logs
```

- source 导入乱码的问题
```
mysql -u root -p --default-character-set=utf8
```

- auto_increment的问题
在表设计自增时，如果想要在中间插入几条记录,先更新表
```
update `xs`.`app_log` set id = id+3 where id>20 order by id desc
```
此时就会造成一个问题，表会增加3个id数据，但是auto_increment 任然停留在原来的位置，这时再添加数据就会报主键错误

#### 8.长度说明
```

-- 长度说明:
-- tinyint(4):
  -- unsigned: -128 - 127
  -- unsigned:    0 - 255
-- smallint(6):
  -- unsigned: -32768 - 32767
  -- unsigned:      0 - 65535
-- mediumint(9):
  -- unsigned: -8388608 - 8388607
  -- unsigned:        0 - 16777215
-- int(11):
  -- unsigned: -2147483648 - 2147483647
  -- unsigned:           0 - 4294967295
-- bigint(20):
  -- unsigned: -9223372036854775808 - 9223372036854775807
  -- unsigned:                    0 - 18446744073709551615
```

#### 9.主从复制
- master
```
$ vi /etc/my.cnf
server-id = 1
log-bin = /var/run/mysqld/mysql-bin

$ /usr/bin/mysql -uroot -ppassword
mysql>show master status;           #slave中需要结果集中的position
```

- slave
```
$ vi /etc/my.cnf
    server-id = 2

$ /usr/bin/mysql -uroot -ppassword
mysql>stop slave;
mysql>CHANGE MASTER TO
      MASTER_HOST='127.0.0.1',
      MASTER_USER='root',
      MASTER_PASSWORD='root',
      MASTER_LOG_FILE='binlog.000001',
      MASTER_LOG_POS=154;
msyql>start slave;
mysql>show slave status;
```

- id冲突
```
# 删掉从库的中相应的id
```

- 从库同步delete语句：not find record
```
# 直接执行跳过错误
mysql>SET GLOBAL SQL_SLAVE_SKIP_COUNTER = 1;   #值可以设置, 跳过指定数量的事务
mysql>stop slave;
mysql>start slave;
```

- memory存储引擎表的同步
```
# 修改存储引擎类型
```

- 修改mysql的配置文件，通过slave_skip_errors参数来跳所有错误或指定类型的错误
```
$ vi /etc/my.cnf
slave-skip-errors=1062,1053,1146    #跳过指定error类型的错误
slave-skip-errors=all               #跳过所有错误
```

#### 10.事务基本要素（ACID）
- 原子性（Atomicity）
- 一致性（Consistency）
- 隔离性（Isolation）
- 持久性（Durability）

#### 11.事务隔离级别
- 脏读：事务A读取了事务B更新的数据，然后B回滚操作，那么A读取到的数据是脏数据
- 不可重复读：事务 A 多次读取同一数据，事务 B 在事务A多次读取的过程中，对数据作了更新并提交，导致事务A多次读取同一数据时，结果 不一致
- 幻读：系统管理员A将数据库中所有学生的成绩从具体分数改为ABCDE等级，但是系统管理员B就在这个时候插入了一条具体分数的记录，当系统管理员A改结束后发现还有一条记录没有改过来，就好像发生了幻觉一样，这就叫幻读
> 注意：不可重复读的和幻读很容易混淆，不可重复读侧重于修改，幻读侧重于新增或删除。解决不可重复读的问题只需锁住满足条件的行，解决幻读需要锁表

    | 事务隔离级别                 | 脏读     | 不可重复读     | 幻读     |
    | -------------------------- | ------- | ------------ | ------- |
    | 读未提交（read-uncommitted） | yes      | yes         | yes      |
    | 读提交（read-committed）     | no      | yes         | yes      |
    | 可重复读（repeatable-read）  | no      | no         | yes      |
    | 串行化（serializable）       | no      | no         | no      |

#### 12.分布式事务
一锁二判三更新，跨数据库的事务支持。保证数据一致性：事务性消息（最终一致性：消息回查，幂等性控制）

- XA两阶段提交
    - 一阶段：事务协调者(事务管理器)给每个参与者(资源管理器)发送Prepare消息
    - 二阶段：如果协调者收到了参与者的失败消息或者超时，直接给每个参与者发送回滚(Rollback)消息；否则，发送提交(Commit)消息
    - 缺点：1.同步阻塞问题。执行过程中，所有参与节点都是事务阻塞型的；2.单点故障。由于协调者的重要性，一旦协调者发生故障，参与者会一直阻塞下去；3.数据不一致。部分参与者发生了局部网络异常或者在发送commit请求过程中协调者发生了故障
- TCC三阶段提交：Try/Confirm/Cancel
    - Try：预留业务资源/数据效验
    - Confirm：确认执行业务操作
    - Cancel：取消执行业务操作
- 为什么不用：对业务有侵入，工程太大，采用用事务性消息，或者定时任务重试机制

#### 13.分库分表
mycat中间件，Spring AOP

- 如何实现：10库100表，销售订单号预留尾号四位，后三位计算库位表位，第四位为扩容预留
- 如何扩容：预留尾号加一位

#### 14.幂等性
多次处理结果是一样的（业务幂等和全局幂等），保证幂等性，可以数据库的唯一索引或者分布式锁等（乐观锁、防重表、分布式锁、Token令牌、支付缓冲区）

#### 15.锁机制
乐观锁，悲观锁（页级锁，行级锁，表级锁：共享锁，排它锁，更新锁，意向锁）。
死锁：互占用对方资源；意向锁上升为独占锁

#### 16.CAP、Base理论
- CAP：一致性(Consistency)、可用性(Availability)、分区容错性(Partition tolerance)，只能实现其中之二
- Base：Basically Available（基本可用）、Soft state（软状态）、Eventually consistent（最终一致性）

#### 17.索引（B+树）
复合索引/最左原则（最左优先）：在创建多列索引时，要根据业务需求，where子句中使用最频繁的一列放在最左边。
like使用索引问题：不已%开头可以用到索引