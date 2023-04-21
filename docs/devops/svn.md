### Svn
版本控制

---

#### Installation
```
apt-get install subversion
```

#### Configuration
```
mkdir -p /home/svn/www
cd /home/svn/www
svnadmin create /home/svn/www   # 创建版本库
cd conf/
vi svnserve.conf
    anon-access = read
    auth-access = write
    password-db = passwd
    authz-db = authz
vi passwd
    [users]
    name = pwd
    name1 = pwd1
vi authz
    [/]
    * = rw      #指定用户什么权限
    name = rw

# 简单配置同步更新：即有版本提交到服务器，服务器部署的代码会自动更新
cd /home/svn/www/hooks/
cp post-commit.tmpl post-commit
chmod +x post-commit
vi post-commit
    #!/bin/sh
    export LANG=en_US.UTF-8
    cd /home/wwwroot/www/
    svn update --username name --password pwd --no-auth-cache --non-interactive

cd /home/wwwroot/
# 实际改成域名
svn checkout svn://localhost/spider --username name --password pwd
```


#### 迁移
```
# 原服务器
svnadmin dump km > km.dump
# 传输至新服务器
scp km.dump root@ip:/home/svn
# 新服务器
svnadmin load km < km.dump
```


#### Start
```
svnserve -d -r /home/svn
```

#### 项目结构
```
# 一般项目结构
# 1.在svn下创建trunk目录，并提交至svn服务器
# 2.Repo-Browser进入svn，选中trunk右键copy to --> branches/dev
# 3.branches/dev --> branch/tag
```

#### 修改仓库地址
```
# 修改仓库URL
# Windows TortoiseSVN客户端：在工作复本的根目录上右键->TortoiseSVN->重新定位(Relocate),然后修改URL
# Mac OS或Linux客户端：
svn sw --relocate svn://old_ip/目录 svn://new_ip/目录
```

#### Exception
```
# Another process is blocking the working copy database, or the underlying filesystem does not support file locking; if the working copy is on a network filesystem, make sure file locking has been enabled on the file server
svn cleanup : break locks
```