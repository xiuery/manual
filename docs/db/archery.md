### archery
一站式的SQL审核查询平台

---

#### Links
- [https://archerydms.com/installation/docker/](https://archerydms.com/installation/docker/)
- [https://github.com/hhyo/archery/releases/](https://github.com/hhyo/archery/releases/)

#### 准备运行配置: 1.9.1
- .env: DATABASE_URL/CACHE_URL/CSRF_TRUSTED_ORIGINS
```
NGINX_PORT=9123

# https://django-environ.readthedocs.io/en/latest/quickstart.html#usage
# https://docs.djangoproject.com/zh-hans/4.1/ref/settings/
DEBUG=false
DATABASE_URL=mysql://archery:password@Host:3306/archery
CACHE_URL=redis://redis:6379/0?PASSWORD=password

# https://docs.djangoproject.com/en/4.0/ref/settings/#csrf-trusted-origins
# 实际改成域名 
CSRF_TRUSTED_ORIGINS=http://127.0.0.1:9123

# https://django-auth-ldap.readthedocs.io/en/latest/
ENABLE_LDAP=false
AUTH_LDAP_ALWAYS_UPDATE_USER=true
AUTH_LDAP_USER_ATTR_MAP=username=cn,display=displayname,email=email

# https://django-q.readthedocs.io/en/latest/configure.html#
Q_CLUISTER_WORKERS=4
Q_CLUISTER_TIMEOUT=60
Q_CLUISTER_SYNC=false
```

- ./archery/settings.py
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'archery', # 数据库名称
        'USER': 'archery', # 数据库用户
        'PASSWORD': 'password', # 数据库密码
        'HOST': 'Host', # 数据库HOST，如果是docker启动并且关联，可以使用容器名连接
        'PORT': '3306',  # 数据库端口
        'OPTIONS': {
            'init_command': "SET sql_mode='STRICT_TRANS_TABLES'", # SQL_MODE，为了兼容select * group by，可以按需调整
            'charset': 'utf8mb4'
        },
        'TEST': {
            'NAME': 'test_archery',
            'CHARSET': 'utf8mb4',
        },
    }
}
```

- ./inception/config.toml 
```
backup_host = "Host"
backup_port = 3306
backup_user = "archery_backup"
backup_password = "password"
```

- docker-compose.yml按需配置
    - redis: 密码修改，注意与.env中保持一致
    - mysql: 这里移除mysql部分，注意修改archery.entrypoint
```
services:
  redis:
    image: redis:5
    container_name: redis
    restart: always
    command: redis-server --requirepass password
    expose:
      - "6379"

  goinception:
    image: hanchuanchuan/goinception
    container_name: goinception
    restart: always
    ports:
      - "4000:4000"
    volumes:
      - "./inception/config.toml:/etc/config.toml"

  archery:
    image: hhyo/archery:v1.9.1
    container_name: archery
    restart: always
    ports:
      - "9123:9123"
    volumes:
      - "./archery/settings.py:/opt/archery/local_settings.py"
      - "./archery/soar.yaml:/etc/soar.yaml"
      - "./archery/docs.md:/opt/archery/docs/docs.md"
      - "./archery/downloads:/opt/archery/downloads"
      - "./archery/sql/migrations:/opt/archery/sql/migrations"
      - "./archery/logs:/opt/archery/logs"
      - "./archery/keys:/opt/archery/keys"
    entrypoint: "dockerize -wait tcp://Host:3306 -wait tcp://redis:6379 -timeout 60s /opt/archery/src/docker/startup.sh"
    env_file:
      - .env
```

#### 首次启动
```
# 启动
cd /opt/archery-1.9.1/src/docker-compose/
docker-compose -f docker-compose.yml up -d

# 表结构初始化
docker exec -ti archery /bin/bash
cd /opt/archery
source /opt/venv4archery/bin/activate
python3 manage.py makemigrations sql  
python3 manage.py migrate 

# 数据初始化
python3 manage.py dbshell<sql/fixtures/auth_group.sql
python3 manage.py dbshell<src/init_sql/mysql_slow_query_review.sql

# 创建管理用户
python3 manage.py createsuperuser

# 退出容器重启
docker restart archery

# 日志查看和问题排查
docker logs archery -f --tail=50
```

#### 必要配置
- [goInception配置](https://archerydms.com/configuration/)

#### [升级](https://archerydms.com/installation/upgrade/)
