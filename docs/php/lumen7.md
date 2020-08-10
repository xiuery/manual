### Lumen7
API

---

#### Links
- [laravel内核分析](https://learnku.com/docs/laravel-kernel)
- [lumen中文文档](https://learnku.com/docs/lumen/6.x)
- [dingo中文文档](https://learnku.com/docs/dingo-api/3.x)
- [dingo官方文档](https://github.com/dingo/api/wiki)
- [UML类图和时序图](https://design-patterns.readthedocs.io/zh_CN/latest/)
- [lumen7.0示例](https://github.com/Jiannei/lumen-api-starter)
- [优化 PHP 和 Laravel 以提高 Web 应用的性能](https://learnku.com/laravel/t/47213)
- [浅谈JWT绕过](https://www.v0n.top/2019/11/01/%E6%B5%85%E8%B0%88JWT%E7%BB%95%E8%BF%87/)

#### 系统设计
- sql事件：记录sql日志，文件或者入库
- sql可控：复杂的sql需要抛弃ORM手写
- request事件：记录api请求
- 图片服务器：api接口
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
     