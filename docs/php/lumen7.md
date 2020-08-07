### Lumen
API

---

#### Links
- [laravel内核分析](https://learnku.com/docs/laravel-kernel)
- [lumen中文文档](https://learnku.com/docs/lumen/6.x)
- [dingo中文文档](https://learnku.com/docs/dingo-api/3.x)
- [dingo官方文档](https://github.com/dingo/api/wiki)
- [UML类图和时序图](https://design-patterns.readthedocs.io/zh_CN/latest/)
- [lumen7.0示例](https://github.com/Jiannei/lumen-api-starter)

#### 系统设计
- sql事件：记录sql日志，文件或者入库
- request事件：记录api请求
- 三方调用登录

#### 环境要求
- PHP >= 7.2
- OpenSSL PHP 拓展
- PDO PHP 拓展
- Mbstring PHP 拓展

#### Installation
```
# lumen安装器
composer global require "laravel/lumen-installer"

# 创建lumen项目
lumen new xs-auth

# 创建.env
cp .env.example .env

# 应用密钥
php artisan key:generate
```

#### artisan命令
```
# 命令列表
php artisan list
```
     