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

