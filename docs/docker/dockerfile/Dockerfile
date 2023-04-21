FROM 10.0.0.119:8082/xiuery/nginx-php-fpm:5.6
LABEL author=xiuery

ENV TZ Asia/Shanghai

# 自定义PHP配置
ENV ETC_PATH_PHP /usr/local/etc
COPY ./docker/php-fpm-56 $ETC_PATH_PHP
RUN rm -f $ETC_PATH_PHP/Dockerfile
# 创建PHP日志目录
ENV LOG_DIR_PHP /usr/local/log/
RUN mkdir -p $LOG_DIR_PHP
#RUN chmod -R 766 $LOG_DIR_PHP

# 自定义nginx配置：更改配置
ENV ETC_PATH_NGINX /etc/nginx
COPY ./docker/nginx/conf $ETC_PATH_NGINX
COPY ./docker/default.conf $ETC_PATH_NGINX/conf.d/

# 项目代码
ENV PROJECT /var/www/html/project
RUN mkdir -p $PROJECT
COPY . $PROJECT
RUN rm -rf $PROJECT/docker \
    && rm -f $PROJECT/Dockerfile \
    && rm -f $PROJECT/*.sql \
    && rm -f $PROJECT/email.html \
    && rm -f $PROJECT/Jenkinsfile \
    && rm -f $PROJECT/README.md \
    && chmod -R 777 $PROJECT

# 工作目录
WORKDIR $PROJECT

EXPOSE 80 9000

# build
# docker build -t="10.0.0.119:8082/project-name:version" .
