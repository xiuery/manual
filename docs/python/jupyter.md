### jupyter
web services for interactive computing across all programming languages

---

#### Links
- [JupyterLab Documentation](https://jupyterlab.readthedocs.io/en/stable/index.html)
- [Jupyter notebook/lab安装及远程访问](https://zhuanlan.zhihu.com/p/166425946)
- [JupyterLab](https://zhuanlan.zhihu.com/p/383005827)

#### 准备运行配置
- start.sh: 弃用
```
#!/bin/bash

# project dir
WORK_DIR="/opt/jupyterlab"
PROJECT_DIR="${WORK_DIR}/project"

# options
LAB_OPTS="--notebook-dir=${PROJECT_DIR}"
LAB_OPTS="${LAB_OPTS} --ip='*'"
LAB_OPTS="${LAB_OPTS} --port=8888"
LAB_OPTS="${LAB_OPTS} --allow-root"
LAB_OPTS="${LAB_OPTS} --no-browser"

/bin/sh -c "jupyter lab $LAB_OPTS"
/bin/sh -c "jupyter lab --generate-config"
```

- Dockerfile
```
# anaconda3
FROM localhost:8002/library/anaconda3:2022.10
LABEL maintainer="xiuery.com@gmail.com"

USER root
ENV TZ Asia/Shanghai
ENV PROJECT /opt/jupyterlab

RUN mkdir -p $PROJECT/project
WORKDIR $PROJECT

RUN conda install -c conda-forge jupyterlab \
    && pip install npm \
    && conda install -c conda-forge nodejs

CMD jupyter lab --notebook-dir=$PROJECT/project --ip='*' --port=8888 --allow-root --no-browser
```

- Build Dockerfile
```
docker build -t="localhost:8002/app/jupyter:3.4.4" .
docker build --no-cache -t="localhost:8002/app/jupyter:3.4.4" .

docker push localhost:8002/app/jupyter:3.4.4
```

- docker-compose
```
version: '3.9'
services:
  jupyter:
    image: localhost:8002/app/jupyter:3.4.4
    container_name: jupyter
    hostname: jupyter
    restart: always
    ports:
      - 8300:8888
    volumes:
      - ./.jupyter:/root/.jupyter
      - ./project:/opt/jupyterlab/project
```

- 启动及配置
```
docker-compose -f docker-compose.yml up -d

# 查看日志及token
docker-compose logs | more
dcoker logs jupyter -f --tail=50

# 生成配置文件
docker exec -ti jupyter /bin/bash
jupyter lab --generate-config

# jupyter web页面生成password
from IPython.lib import passwd
passwd()

# 将生成的加密的串更新到配置文件
# /opt/jupyter/.jupyter/jupyter_lab_config.py
c.ServerApp.password = 'sha1:passwd:password'

# 重启
docker-compose stop
docker-compose start

# 停止
docker-compose down -v 
```
