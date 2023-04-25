### sed
sed命令

---

#### 常用命令
```
a\ 在当前行下面插入文本
i\ 在当前行上面插入文本
c\ 把选定的行改为新的文本
d  删除，删除选择的行
D  删除模板块的第一行
s  替换指定字符
h  拷贝模板块的内容到内存中的缓冲区
H  追加模板块的内容到内存中的缓冲区
g  获得内存缓冲区的内容，并替代当前模板块中的文本
G  获得内存缓冲区的内容，并追加到当前模板块文本的后面
l  列表不能打印字符的清单
n  读取下一个输入行，用下一个命令处理新的行而不是用第一个命令
N  追加下一个输入行到模板块后面并在二者间嵌入一个新行，改变当前行号码
p  打印模板块的行。 P(大写) 打印模板块的第一行
q  退出Sed
b  lable 分支到脚本中带有标记的地方，如果分支不存在则分支到脚本的末尾
r  file 从file中读行
t  label if分支，从最后一行开始，条件一旦满足或者T，t命令，将导致分支到带有标号的命令处，或者到脚本的末尾
T  label 错误分支，从最后一行开始，一旦发生错误或者T，t命令，将导致分支到带有标号的命令处，或者到脚本的末尾
w  file 写并追加模板块到file末尾
W  file 写并追加模板块的第一行到file末尾
!  表示后面的命令对所有没有被选定的行发生作用
=  打印当前行号
#  把注释扩展到下一个换行符以前
```

#### 替换标记
```
g 表示行内全面替换
p 表示打印行
w 表示把行写入一个文件
x 表示互换模板块中的文本和缓冲区中的文本
y 表示把一个字符翻译为另外的字符（但是不用于正则表达式）
\1 子串匹配标记
& 已匹配字符串标记
```

#### 元字符集
```
^ 匹配行开始，如：/^sed/匹配所有以sed开头的行
$ 匹配行结束，如：/sed$/匹配所有以sed结尾的行
. 匹配一个非换行符的任意字符，如：/s.d/匹配s后接一个任意字符，最后是d
* 匹配0个或多个字符，如：/*sed/匹配所有模板是一个或多个空格后紧跟sed的行
[] 匹配一个指定范围内的字符，如/[ss]ed/匹配sed和Sed
[^] 匹配一个不在指定范围内的字符，如：/[^A-RT-Z]ed/匹配不包含A-R和T-Z的一个字母开头，紧跟ed的行
\(..\) 匹配子串，保存匹配的字符，如s/\(love\)able/\1rs，loveable被替换成lovers
& 保存搜索字符用来替换其他字符，如s/love/**&**/，love这成**love**
\< 匹配单词的开始，如:/\ 
\> 匹配单词的结束，如/love\>/匹配包含以love结尾的单词的行
x\{m\} 重复字符x，m次，如：/0\{5\}/匹配包含5个0的行
x\{m,\} 重复字符x，至少m次，如：/0\{5,\}/匹配至少有5个0的行
x\{m,n\} 重复字符x，至少m次，不多于n次，如：/0\{5,10\}/匹配5~10个0的行
```

#### test.txt
```
test.txt:
HELLO LINUX!
Linux is a free unix-type opterating system.
This is a linux testfile!
Linux test
Google
Taobao
Runoob Runoob
Tesetfile
Wiki
runoob.
google.
taobao.
facebook.
zhihu-
weibo-
```

#### 详解
```
# 第四行后添加一行, 不会写入文件
sed -e 4a\5555555555 test.txt
# 带空格
sed -e 4a\555555\ 55 test.txt
# 第四行前添加一行
sed -e 4i\4444444444 test.txt

# 第2行删除
nl test.txt | sed -e '2d'
# 第2~5行删除
nl test.txt | sed -e '2,5d'
# 删除第3到最后一行
nl test.txt | sed -e '3,$d' 
# 删除最后一行
nl test.txt | sed -e '$d' 

# 第二行后(即加在第三行) 
nl test.txt | sed -e '2a 222222 - 33333'
# 第二行前
nl test.txt | sed -e '2i 111111 - 222222'
# 添加多行
nl test.txt | sed -e '2a 2222222222222 2\
3333333333333333'

# 替换多行
nl test.txt | sed -e '2,5c No 2-5 number'
# 替换一行
nl test.txt | sed -e '2,2c New 222222222'

# 仅列出第5-7行
nl test.txt | sed -n '5,7p'

# 搜索
nl test.txt | sed -n '/oo/p'
# 搜索删除行
nl test.txt | sed -e '/oo/d'
搜寻并执行命令: 花括号中的一组命令，每个命令之间用分号分隔
nl test.txt | sed -n '/oo/{s/oo/kk/;p;q}'

# 已匹配字符串标记&
sed 's/\w\+/[&]/g' test.txt
```

#### 查找与替换
```
# 每行第一次出现的 oo 用字符串 kk 替换
sed -e 's/oo/kk/' test.txt

# 对文件中所有符合的字符串都被替换
sed -e 's/oo/kk/g' test.txt

# 文件操作(危险动作): -i
sed -i 's/oo/kk/g' test.txt
# 批量操作
sed -i 's/oo/kk/g' ./test*

# 提取内容
echo 'inet addr:192.168.1.100 Bcast:192.168.1.255 Mask:255.255.255.0' | sed 's/^.*addr://g' | sed 's/Bcast.*$//g'

# 多点编辑
nl test.txt | sed -e '3,$d' -e 's/HELLO/RUNOOB/'

# 直接修改文件内容(危险动作)
sed -i 's/\.$/\!/g' test.txt

# 最后一行加入: $a
sed -i '$a # This is new last line' test.txt
```