# learn shell quickly

## BASIC

### printf
```bash
[root@AY14071615564053440bZ ~]# printf "hello %s %s", ss ee
hello ss ee,
[root@AY14071615564053440bZ ~]# printf "hello %s %s" ss ee
hello ss ee
[root@AY14071615564053440bZ ~]# printf "hello %s %s"ss ee
hello ee ss
[root@AY14071615564053440bZ ~]# printf "hello %s %s" ss ee
hello ss ee
```

### 转义序列
```bash
	\r 回车
    \t 水平制表符
    \v 垂直制表符
    \\ 反斜杠
    \0ddd 将字符表示成1到3位的八进制数值
```

### I/O重定向
```bash
#!/usr/bin/bash
#将所有的font*.css文件删除回车后追加到test1.txt
# `<` 输入  `>`覆盖/新建输出  '>>'追加输出
for f in font*.css
do
	tr -d '\r' < $f >> test1.txt
done
```

### stty
```bash
	stty -echo 关闭显示字符输入
    stty echo 打开显示
```

### export
```
设置或显示环境变量，让变量在所有子shell中都有拷贝
```

### 命令行参数
```bash
#!usr/bin/bash

echo first args $1
echo second args ${2}

=======================

[root@AY14071615564053440bZ bin]# sh test2.sh 1 2 3
first args 1
second args 2
```

### 简单执行跟踪(-x)
```bash
[root@AY14071615564053440bZ bin]# sh -x test2.sh 1 2 3 4
+ echo first args 1
first args 1
+ echo second args 2
second args 2

=======================
或者在文件中
set -x
=======================

[root@AY14071615564053440bZ bin]# cat test3.sh
#!usr/bin/bash

set -x #打开跟踪
echo 1st echo

set +x #关闭跟踪
echo 2nd echo

[root@AY14071615564053440bZ bin]# sh test3.sh
+ echo 1st echo
1st echo
+ set +x
2nd echo
```

## 查找替换

### grep正则匹配
```bash
# -F 固定匹配 -E 正则匹配
root@AY14071615564053440bZ bin]# who | grep -F o
root     pts/2        2015-11-21 13:33 (58.33.237.167)
[root@AY14071615564053440bZ bin]# who | grep -E \d
[root@AY14071615564053440bZ bin]# who | grep -E [0-9]+
root     pts/2        2015-11-21 13:33 (58.33.237.167)
[root@AY14071615564053440bZ bin]# echo hhhh | grep -E [[:lower:]]{4}
hhhh

=====================
正则模式字符

\ 转义
. 单字符
* 任何数目字符
^ 起始
$ 结尾
[] 括号内任一字符，(-)连字符表示区间,(^)在括号内表示不在括号内的字符
{n,m} 前面字符出现n到m次
() 组合多个模式为一个模式
+ 1个或多个
? 0个或1个
| 或

====================
POSIX字符集

[:alnum:]    数字
[:alpha:]    字母
[:blank:]    空格与tab
[:cntrl:]    控制字符
[:digit:]    数字
[:graph:]    非空格
[:lower:]    小写字母
[:upper:]    大写字母
[:print:]    可显示字符
[:punct:]    标点字符
[:space:]    空白
[:xdigit:]  十六进制字符

====================
GNU正则表达式运算符

\w 字母
\W 非字母
\<\> 匹配单词起始与结尾
\b 单词首尾空字符串(awk 中\b表示后退 ， 用\y代替)
\B 两个单词字符之间的空字符串
\' \` 相当于^ $
```

### sed
```bash
#替换
[root@AY14071615564053440bZ bin]# echo hhhh | sed 's/hh/oooo88999/'
oooo88999hh
[root@AY14071615564053440bZ bin]# echo hhhh | sed 's/hh/oooo88999/g'
oooo88999oooo88999
[root@AY14071615564053440bZ bin]# echo hhhh | sed 's/HH/oooo88999/i'
oooo88999hh
[root@AY14071615564053440bZ bin]# echo hhhh | sed 's/HH/oooo88999/'
hhhh
[root@AY14071615564053440bZ bin]# echo hhhh | sed 's/HH/oooo88999/ig'
oooo88999oooo88999
[root@AY14071615564053440bZ bin]# echo hhhh | sed 's/HH/oooo88999/ig2'
hhoooo88999

====================
获取特定行
#获取1~3行
[root@AY14071615564053440bZ bin]# sed -n '1,3p' test1.sh
#!usr/bin/bash

printf "first shell script"

[root@AY14071615564053440bZ bin]# sed -n '1,4p' test3.sh
#!usr/bin/bash

set -x
echo 1st echo

====================
#冒号隔开要查找的模式，分号扮演s命令定界符
[root@AY14071615564053440bZ ~]# grep root /etc/passwd
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
root@AY14071615564053440bZ ~]# sed -n '\:root: s;;congtang;p' /etc/passwd
congtang:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/congtang:/sbin/nologin

====================
#正则表达式
[root@AY14071615564053440bZ ~]# echo hello123 | sed 's/[a-z]/333/'
333ello123 
```

### cut
```bash
# -d 分隔符 -f 选取列
[root@AY14071615564053440bZ ~]# cut -d : -f 1,2 /etc/passwd
root:x
bin:x
daemon:x
adm:x
lp:x
sync:x
shutdown:x
halt:x
mail:x
syss:x
uucp:x

===================
#-c 以字符分割
root@AY14071615564053440bZ ~]# cut -c 1-5 /etc/passwd
root:
bin:x
daemo
```

### join
```bash
[root@AY14071615564053440bZ bin]# cat name_sex.txt
#name  sex
tom 男
jim 男
lucy 女
[root@AY14071615564053440bZ bin]# cat name_height.txt
#name height
tom 167
jim 176
lucy 156
[root@AY14071615564053440bZ bin]# vim merge_data.sh
[root@AY14071615564053440bZ bin]# cat merge_data.sh
#!/usr/bin/bash

#merge name_sex and name_height

#删除注释并排序文件
sed '/^#/d' name_sex.txt | sort > name_sex_sort.txt
sed '/^#/d' name_height.txt | sort > name_height_sort.txt

#以第一个键作为基准结合数据
join name_sex_sort.txt name_height_sort.txt

#删除缓存文件
rm name_sex_sort.txt name_height_sort.txt
[root@AY14071615564053440bZ bin]# sh merge_data.sh
jim 男 176
lucy 女 156
tom 男 167
```

### awk重排
```bash
#-F分割符 $1表示列字段 $0表示全部字段
[root@AY14071615564053440bZ bin]# awk -F: '{print $1,$3}' /etc/passwd
root 0
bin 1
daemon 2
adm 3
lp 4
sync 5
shutdown 6
halt 7

========================
#-v设置输出分隔符
[root@AY14071615564053440bZ bin]# awk -F: -v 'OFS=#' '{print $1,$3}' /etc/passwd
root#0
bin#1
daemon#2
adm#3
lp#4
sync#5
shutdown#6

=======================
#printf 格式化输出
root@AY14071615564053440bZ bin]# awk -F: '{printf "hello %s,%s\n",$1,$3}' /etc/passwd
hello root,0
hello bin,1
hello daemon,2
hello adm,3
hello lp,4
hello sync,5
```

## 程序结构

### 变量
```bash
export 修改导出环境变量
readonly 只读变量
unset 删除变量或函数
```

### sleep
```bash
#等待10s
sleep 10
```

### 展开运算符
```bash
${var:-word} 相当于 return var !=NULL ? var : word
${var:word}  相当于 var = var !=NULL ? var : word
${var:?word} 相当于if(var != NULL){return var;}else{ print var:word; exit;}
${var:+word} 相当于 return var == NULL ? var : word
eg: filename=${1:-/src/data/access.log}
```

### 位置参数
```bash
$1,$2...${10},${11} 位置参数
$# 参数总数
$@,$* 所有参数
"$*" = "$1 $2 $3 ... "
"$@" = "$1" "$2" ...
```

### 运算符
```bash
++,-- 递增，递减
+,-,*,/,% 加减乘除余
==，！=，>,<,>=,<= 比较
&&,||逻辑比较
？：条件表达式
=，+=，-=，*=，、/=,%= 赋值
+,-,!,~,>>,<<,&,|,^,>>=,<<=,|=,&=,^= 位运算
eg:
echo $((3 && 4))
```

### 程序退出状态
```bash
$? 程序退出状态
0 成功退出
1-125 命令不成功的退出
126 命令找到了，文件无法执行
127 命令找不到
>128 命令因收到信号死亡
>255 会/256取余数
eg:
exit 88
```

### if-elif-else-fi 语句
```bash
#!/bin/bash

if [ "$1"x = "a"x ];then
    echo "hello a"
elif [ "$1"x = "b"x ];then
    echo "hello b"
else
    echo 'hello undifinend'
fi
```
