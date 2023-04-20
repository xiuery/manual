### metabase
Business Intelligence

---

#### Links
- [Installing Metabase](https://www.metabase.com/docs/latest/installation-and-operation/installing-metabase)
- [Configuring the Metabase application database](https://www.metabase.com/docs/latest/installation-and-operation/configuring-application-database)
- [Environment variables](https://www.metabase.com/docs/latest/configuring-metabase/environment-variables)


#### 准备运行配置
- 镜像
```
docker pull metabase/metabase:v0.46.1
```

- 创建日志目录
```
cd /opt/metabase
mkdir ./logs
chmod -R 777 ./logs
```

- log4j2.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
  <properties>
    <property name="LOGFILE_HOME">/var/log/metabase</property>
  </properties>
  <Appenders>
    <Console name="STDOUT" target="SYSTEM_OUT" follow="true">
      <PatternLayout pattern="%date %level %logger{2} :: %message%n%throwable">
        <replace regex=":basic-auth \\[.*\\]" replacement=":basic-auth [redacted]"/>
      </PatternLayout>
    </Console>

    <!-- This file appender is provided as an example -->
    <!-- 输出到文件配置 -->
    <RollingFile name="FILE" fileName="${LOGFILE_HOME}/metabase.log" filePattern="${LOGFILE_HOME}/metabase.log.%i">
      <Policies>
        <SizeBasedTriggeringPolicy size="20 MB"/>
      </Policies>
      <DefaultRolloverStrategy max="2"/>
      <PatternLayout pattern="%d [%t] %-5p%c - %m%n">
        <replace regex=":basic-auth \\[.*\\]" replacement=":basic-auth [redacted]"/>
      </PatternLayout>
    </RollingFile>
  </Appenders>

  <Loggers>
    <Logger name="metabase" level="INFO" additivity="true">
      <!-- 输出到文件 -->
      <AppenderRef ref="FILE"/>
    </Logger>
    <Logger name="metabase-enterprise" level="INFO" additivity="true">
      <AppenderRef ref="FILE"/>
    </Logger>
    <Logger name="metabase.metabot" level="DEBUG" additivity="true">
      <AppenderRef ref="FILE"/>
    </Logger>
    <Logger name="metabase.plugins" level="DEBUG" additivity="true">
      <AppenderRef ref="FILE"/>
    </Logger>
    <Logger name="metabase.server.middleware" level="DEBUG" additivity="true">
      <AppenderRef ref="FILE"/>
    </Logger>
    <Logger name="metabase.query-processor.async" level="DEBUG" additivity="true">
      <AppenderRef ref="FILE"/>
    </Logger>
    <Logger name="com.mchange" level="ERROR" additivity="true">
      <AppenderRef ref="FILE"/>
    </Logger>
    <Logger name="org.quartz" level="INFO" additivity="true">
      <AppenderRef ref="FILE"/>
    </Logger>
    <Logger name="liquibase" level="ERROR" additivity="true">
      <AppenderRef ref="FILE"/>
    </Logger>
    <Logger name="net.snowflake.client.jdbc.SnowflakeConnectString" level="ERROR" additivity="true">
      <AppenderRef ref="FILE"/>
    </Logger>

    <Logger name="org.eclipse.jetty" level="INFO" additivity="true">
      <AppenderRef ref="FILE"/>
    </Logger>

    <Root level="INFO">
       <!-- 输出到控制台 -->
      <AppenderRef ref="STDOUT"/>
    </Root>
  </Loggers>
</Configuration>
```

- docker-compose
```
version: '3.9'
services:
  metabase:
    image: metabase/metabase:v0.46.1
    container_name: metabase
    hostname: metabase
    ports:
      - 3000:3000
    volumes:
      - ./log4j2.xml:/app/log4j2.xml
      - ./logs:/var/log/metabase
      - /dev/urandom:/dev/random:ro
      - /data/metabase:/metabase-data
    environment:
      JAVA_TIMEZONE: Asia/Shanghai
      JAVA_OPTS: -Dlog4j.configurationFile=file:///app/log4j2.xml
      MB_DB_TYPE: mysql
      MB_DB_DBNAME: metabase
      MB_DB_PORT: 3306
      MB_DB_USER: metabase
      MB_DB_PASS: metabase
      MB_DB_HOST: localhost
```

- 启动与停止
```
docker-compose -f docker-compose.yml up -d

# 查看日志
docker-compose logs | more
dcoker logs metabase -f --tail=50
tail -f ./logs/metabase.log

# 停止
docker-compose down -v
```

