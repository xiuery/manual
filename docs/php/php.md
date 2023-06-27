### PHP
PHP安装及包管理composer

---

#### Links
- [Install](https://www.php.net/manual/en/install.php)
- [composer](https://getcomposer.org/)
- [依赖包](https://packagist.org/)
- [VC++](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170)

#### php
- 版本选择: 

- nginx + php：需要单独安装nginx和php-fpm，启动两个进程

- apache + php

#### Windows + Nginx + PHP
- 下载：[https://windows.php.net/downloads/releases/archives/](https://windows.php.net/downloads/releases/archives/)

- 安装依赖：选择对应的VC++版本(vcredist_x64.exe)

- 解压，安装，设置环境变量

- php.ini
```
extension_dir = "ext"

cgi.force_redirect = 1

cgi.fix_pathinfo = 1

cgi.rfc2616_headers = 1
```

- 启动：php-cgi -b 127.0.0.1:9000 [-c php.ini]

- 下载nginx、配置并启动
```
location ~ \.php$ {
   root           html;
   fastcgi_pass   127.0.0.1:9000;
   fastcgi_index  index.php;
   fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
   include        fastcgi_params;
}
```

- windows启动脚本
```
# 下载http://redmine.lighttpd.net/attachments/660/RunHiddenConsole.zip

@ECHO OFF

SET RunHiddenConsole=C:\tools\RunHiddenConsole\RunHiddenConsole.exe
SET PHP_HOME=C:\Program Files\php-5.6.40-nts
SET NGINX_HOME=C:\Program Files\nginx-1.14.2

ECHO Starting PHP FastCGI(%PHP_HOME%)...
"%PHP_HOME%\php.exe" -v
%RunHiddenConsole% "%PHP_HOME%\php-cgi.exe" -b 127.0.0.1:9000
ECHO Running PHP FastCGI

TIMEOUT /T 1

ECHO Starting Nginx(%NGINX_HOME%)...
CD "%NGINX_HOME%"
"%NGINX_HOME%\nginx.exe" -v
%RunHiddenConsole% "%NGINX_HOME%\nginx.exe"
ECHO Running Nginx

TIMEOUT /T 10
```

#### composer
```
# Linux / Unix / macOS
php composer-setup.php --install-dir=bin --filename=composer

# Windows
https://getcomposer.org/Composer-Setup.exe
# or 
# cmd.exe:
echo @php "%~dp0composer.phar" %*>composer.bat
# PowerShell:
Set-Content composer.bat '@php "%~dp0composer.phar" %*'
```

