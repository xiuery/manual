### PHP
PHP安装及包管理composer

---

#### [php](https://www.php.net/manual/en/install.php)
- 版本选择

- nginx+php：需要单独安装nginx和php-fpm，启动两个进程

- apache+php

#### [composer](https://getcomposer.org/)
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

#### 依赖包
https://packagist.org/

