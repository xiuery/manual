### Laravel7
Laravel7 + dingo/api + tymon/jwt-auth

---

#### Links
- [laravel加载流程分析](https://github.com/xiaohuilam/laravel)
- [laravel内核分析](https://learnku.com/docs/laravel-kernel)
- [dingo中文文档](https://learnku.com/docs/dingo-api/3.x)
- [dingo官方文档](https://github.com/dingo/api/wiki)
- [UML类图和时序图](https://design-patterns.readthedocs.io/zh_CN/latest/)
- [优化 PHP 和 Laravel 以提高 Web 应用的性能](https://learnku.com/laravel/t/47213)
- [浅谈JWT绕过](https://www.v0n.top/2019/11/01/%E6%B5%85%E8%B0%88JWT%E7%BB%95%E8%BF%87/)

#### 系统设计
- sql事件：记录sql日志，文件或者入库
- sql可控：复杂的sql需要抛弃ORM手写
- request事件：记录api请求
- 图片服务器：api接口
- 三方调用登录

#### 环境要求
- PHP >= 7.2.5
- BCMath PHP 拓展
- Ctype PHP 拓展
- Fileinfo PHP 拓展
- JSON PHP 拓展
- Mbstring PHP 拓展
- OpenSSL PHP 拓展
- PDO PHP 拓展
- Tokenizer PHP 拓展
- XML PHP 拓展

#### Installation
```
# laravel安装器
composer global require laravel/installer

# 创建laravel项目
laravel new xs-auth

# 创建.env
cp .env.example .env

# 应用密钥
php artisan key:generate
```

#### artisan命令
```
# 命令列表
php artisan list
# 生成应用密匙
php artisan key:generate
# 生成jwt密匙
php artisan jwt:secret
# 路由列表
php artisan api:routes
```

#### dingo/api & tymon/jwt-auth
- 安装
```
composer require dingo/api
composer require tymon/jwt-auth
```

- 生成配置文件
```
# 在config目录下生成 api.php & jwt.php
php artisan vendor:publish --provider="Dingo\Api\Provider\LaravelServiceProvider"
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
```

- .env配置
```
# api配置
API_STANDARDS_TREE=x
API_SUBTYPE=xs-auth
#API_PREFIX=api
API_DOMAIN=auth.xiuery.com
API_VERSION=v1
API_NAME=xs-auth
#API_CONDITIONAL_REQUEST=false
#API_STRICT=false
#API_DEBUG=true

# jwt配置
JWT_SECRET=defI1HPh4E2dHM2YZrYMmZDVoff3kwEQ
JWT_TTL=86300
JWT_REFRESH_TTL=20160
```

- 生成jwt密匙
```
php artisan jwt:secret
```

- api && auth配置
```
# api.php：中间件需要此项配置
# 否则报错：Failed to authenticate because of bad credentials or an invalid authorizatio
'auth' => [
    'jwt' => 'Dingo\Api\Auth\Provider\JWT',
]

# auth.php：
# 1.guard改为api
'defaults' => [
    'guard' => 'api',
    'passwords' => 'users',
]

# 2.api.driver改为jwt
'guards' => [
    'web' => [
        'driver' => 'session',
        'provider' => 'users',
    ],

    'api' => [
        'driver' => 'jwt',
        'provider' => 'users',
        'hash' => false,
    ],
]

# 3.model改为映射user model
'providers' => [
    'users' => [
        'driver' => 'eloquent',
        'model' => App\Models\User::class,
    ],

    // 'users' => [
    //     'driver' => 'database',
    //     'table' => 'users',
    // ],
]
```

- 添加router端点
```
$api = app('Dingo\Api\Routing\Router');

$api->version('v1', [
    'namespace' => 'App\Http\Controllers',
], function ($api) {
    // 生成token
    $api->post('auth', 'AuthorizationController@store')->name('api.auth.store');

});
```

- App\Models\User
```
# 
class User extends Authenticatable implements JWTSubject
{
    // 指定认证的字段名称
    protected $username = 'username';

    public function getJWTIdentifier()
    {
        return $this->getKey();
    }

    public function getJWTCustomClaims()
    {
        return [];
    }
}
```

- authorization
```

namespace App\Http\Services;

use App\Models\Authorization;
use Illuminate\Support\Facades\Auth;

class AuthorizationService extends BaseService
{
    private $authorization;

    public function __construct(Authorization $authorization)
    {
        parent::__construct();
        $this->authorization = $authorization;
    }

    public function store($request)
    {
        if (! $token = Auth::attempt($request))
        {
            abort(401, 'auth failed');
        }

        $this->authorization->setToken($token);

        return $this->authorization;
    }
}
```

#### 系统权限
- 路由权限：防止恶意用户直接通过token请求api
- 前端权限：页面禁止操作或者隐藏操作按钮
- 业务权限：防止恶意用户直接通过token请求api

#### 状态码
- 200：成功
- 201：资源创建成功
- 401：认证失败
- 404：资源未找到
- 405：Method Not Allowed
- 422：数据校验错误
- 500：内部错误

#### 表单校验
- required：不能为空的字段必须要该字段
```
足以下条件之一，则字段被视为「空」：
    值为 null 
    值为空字符串
    值为空数组或空 Countable 对象
    值为无路径的上传文件
```
- exists:table[,column]
```
验证的字段必须存在于给定的数据库表中
使用场景：用户名登录，用户名必须存在用户表中
```

- nullable：
```
验证字段可以为 null
使用场景：可以包含空值的字符串和整数
```

- exclude_if：字段包含特定值跳过验证
```
使用场景：pid为0时不需要验证pid存在
exclude_if:pid,0|required|numeric|exists:xs_role
```

#### 文件上传

- 配置目录
```
php artisan storage:link
# windows
mklink /d public\\storage storage\\app\\public
# linux
ln -sr storage/app/public public/storage
```

