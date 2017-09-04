# bash 跟着敲

## 1. 硬件、内核、shell

下面这张摘自鸟哥的图可以很好的说明
![](./img/shell.jpg)

## 2. 命令行

### 1. 基本操作

* export

展示全部的环境变量，如果你想获取某个特殊的变量，用 `echo $变量名`
![](./img/export.jpg)
![](./img/echo.jpg)

* whatis

展示用户命令，系统调用、库函数等
![](./img/whatis.png)

* whereis

搜索可执行文件、源文件
![](./img/whereis.png)

* which

在环境变量中搜索可执行文件，并打印完整路径
![](./img/which.jpg)

* clear

清空屏幕
![](./img/clear.png)

### 2. 文件操作

* cat

在屏幕上显示文本文件
![](./img/cat.jpg)

* chmod

可以改变文件和目录的读、写、执行权限
[linux 中的用户、组、文件][1]

* cp

复制文件
![](./img/copy.jpg)

* diff

比较文件，我在上面复制的文件中加了一行diff
![](./img/diff.jpg)

* find

查找文件，可以通过正则来查
![](./img/find.jpg)

* head

查看文件前10行
![](./img/head.jpg)

* ls

显示所有文件，`-l`显示长格式化 `-a`显示包括隐藏文件
![](./img/ls.jpg)

* mv

移动文件，同时也可以重命名文件
![](./img/mv.jpg)

* rm

删除文件，`-r`删除目录 `-f`强制删除
![](./img/rm.jpg)

* touch

创建新文件
![](./img/touch.jpg)

### 3. 文本操作

* awk

非常有用的一个文本处理命令，逐行运行，默认使用空格分割，`-F`表示分割的样式
![](./img/etc:passwd.jpg)
![](./img/awk.jpg)

* grep

匹配正则表达式的文本行，并输出，`-E`正则模糊匹配 `-F`精确字母匹配 `-w`精确单词匹配
![](./img/grep.jpg)
![](./img/grep_w.jpg)

* sed

正则替换
![](./img/sed.jpg)

* sort

排序
![](./img/sort.jpg)

* wc

统计文本行数，单词数，字符数
![](./img/wc.jpg)

### 4. 目录操作

* cd

进入目录

* mkdir

创建目录

* pwd

当前目录的路径
![](./img/mkdir.jpg)

### 5. SSH,系统信息，网络操作

* cal

月历
![](./img/cal.jpg)

* date

当前日期和时间
![](./img/date.jpg)

* df

磁盘使用情况

* du

文件或目录使用情况
![](./img/du.jpg)

* ps

列出你的进程
![](./img/ps.jpg)

* ssh

远程连接
![](./img/ssh.jpg)

* top

列出当然活动进程
![](./img/top.jpg)

## 3. 语法

### 1 条件判断

#### 1.1 test判断语句

test是关键字，表示判断；EXPRESSION是被判断的语句。

![](./img/testEx.jpg)

`echo $?` 输出判断结果，0表示成功，其他表示失败。
![](./img/test.jpg)

#### 1.2 []条件判断

中括号的左右扩弧和EXPRESSION之间都必须有空格！
![](./img/[1].jpg)

文件类型判断
![](./img/[2].jpg)
字符串和数字判断
![](./img/string-integer.jpg)

### 2 if then else语句

例子1：判断文件README.md是不是文件

```sh
#!/bin/bash

if [ -f ../README.md ];then
echo 'file exist'
else
echo 'file not exist'
fi

exit 0
```

例子2：提示用户输入值，如果大于0输出’正数’，小于0输出’负数‘，等于0输出’零‘

```sh
#!/bin/bash

# 提示用户输入一个值
echo -n "请输入一个数字："

# 保存用户输入的值到num中
read num

if [ "$num" -gt 0 ];then
echo '正数'
elif [ "$num" -lt 0 ];then
echo '负数'
else
echo '零'
fi

exit 0
```

### 3 case语句

例子：提示用户输入Y/y或N/n。若输入Y/y，则输出“我们约起来吧”；若输入N/n,则输出“不约，再见”；否则，“输入有误”

```sh
#!/bin/bash

echo -n "你单身吗？(y/n)"

read val

case $val in
Y|y)
echo "我们约起来吧"
;;
N|n)
echo "不约，再见"
;;
*)
echo "输入有误"
;;
esac

exit 0
```

### 4 for循环

例子1：输出当前文件夹的一级子目录中文件名字

```sh
#!/bin/bash

# 将ls的结果保存到变量CUR_DIR中
CUR_DIR=`ls`

# 显示ls的结果
echo $CUR_DIR

for val in $CUR_DIR
do
if [ -f $val ];then
echo "FILE: $val"
fi
done

exit 0
```

例子2：输出1-10之间数字的总和

```sh
#!/bin/bash

sum=0
for ((i=1;i<10;i++))
do
((sum=$sum+$i))
done

echo "sum=$sum"

exit 0
```

### 5 while循环

例子：从0开始逐步递增，当数值等于5时，停止递增

```sh
#!/bin/bash

val=0

while [ "$val" -lt 5 ]
do
echo "val=$val"
((val++))
done

exit 0
```

### 6 使用break和continue控制循环

break命令允许跳出循环

continue命令类似于 break命令,只有一点重要差别,它不会跳出循环,只是跳过这个循环步。

例子1：[break应用]从0开始逐步递增，当数值等于5时，停止递增。

```sh
#!/bin/bash

# 设置起始值为0
val=0

while true
do
if [ "$val" -eq "5" ];then
# 如果val=5，则跳出循环
break;
else
# 输出数值
echo "val=$val"
# 将数值加1
((val++))
fi
done

exit 0
```

例子2：[continue应用]从0开始逐步递增到10：当数值为5时，将数值递增2；否则，输出数值

```sh
#!/bin/bash

# 设置起始值为0
val=0

while [ "$val" -le "10" ]
do
if [ "$val" -eq "5" ];then
# 如果val=5，则将数值加2
((val=$val+2))
continue;
else
# 输出数值
echo "val=$val"
# 将数值加1
((val++))
fi
done

exit 0
```

## 4. 数组

### 1 数组定义

* array=(10 20 30 40 50)

一对括号表示是数组，数组元素用“空格”符号分割开。引用数组时从序号0开始。

* 除了上面的定义方式外，也可以单独定义数组：

```sh
array[0]=10
array[1]=20
array[2]=30
array[3]=40
array[4]=50
```

* var="10 20 30 40 50"; array=($var)

### 2 数组操作

[数组操作][2]

## 5.函数

[函数实例][3]

## 6.数值运算

```sh
数值元算主要有4种实现方式：(())、let、expr、bc。
工作效率：(()) == let > expr > bc**
(())和let是bash内建命令，执行效率高；而expr和bc是系统命令，会消耗内存，执行效率低。
(())、let和expr只支持整数运算，不支持浮点运算；而bc支持浮点运算。
```

[数值运算][4]
实例1：用4中方式实现`3*(5+2)`

```sh
#!/bin/bash

# (())
val1=$((3*(5+2)))
echo "val1=$val1"

# let
let "val2=3*(5+2)"
echo "val2=$val2"

# expr
val3=`expr 3 \* \(5+2\)`
echo "val3=$val3"

# bc
val4=`echo "3*(5+2)"|bc`
echo "val4=$val4"

exit 0
```

实例3：5/3浮点运算，保留3位小数

```sh
#!/bin/bash

# bc 实现5/3浮点运算，保留3位小数
val5=`echo "scale=3; 5/3"|bc`
echo "val5=$val5"

exit 0
```

## 7. 字符运算

[字符运算][5]

## 8.bash调试

* bash [-nvx] scripts.sh

```sh
选项与参数:
-n :不要执行 script,仅查询语法的问题;
-v :再执行 sccript 前,先将 scripts 的内容输出到屏幕上;
-x :将使用到的 script 内容显示到屏幕上,这是很有用的参数!
```

实例：想要执行bash脚本，并查看bash的调用流程，可以通过以下命令：

`bash -x test.sh`

[1]:http://omgzui.pub/linux
[2]:./dir/array.sh
[3]:./dir/function.sh
[4]:./dir/operation.sh
[5]:./dir/string.sh