### Python
python 常用操作

---

#### Links
- [whl安装包](https://www.lfd.uci.edu/~gohlke/pythonlibs/)

#### Installation
```
# 下载安装包，解压
# configure
./configure --prefix=/usr/local/python3
# make
make && make install

# 安装常见错误
# ModuleNotFoundError: No module named '_ctypes'
yum install -y libffi-devel
make && make install
```
