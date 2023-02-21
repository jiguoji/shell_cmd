[TOC]

# Shell cmd

## 1. [] and ^ use:

- [ 0-9 ] —— []表示方括号里面的任意一个字符，- 表示一个范围， 0-9 表示了0到9的所有数字字符，也就是任意的数字字符。

- grep  ‘ ^# ’  XXXXX —— ^表示以某字符开头，^# 表示以#开头的行

- grep  ‘ [ ^0-9 ] ’  XXXXX —— ==**[]内的^表示“ 非 ”， [ ^0-9 ]表示不是数字字符**== 


- grep  ‘ ^ [ ^0-9 ] ’  XXXXX —— 表示不是数字的字符开头的行

```
[root@SPR-4 shell_test]# ls
1  2.log  ov  pt_1.log  tf.log
[root@SPR-4 shell_test]# ls |grep '[0-9]'              #取含有数字
1
2.log
pt_1.log
[root@SPR-4 shell_test]# ls |grep '[^0-9]'             #取不是数字字符
2.log
ov
pt_1.log
tf.log
[root@SPR-4 shell_test]# ls |grep '^[0-9]'             #取以数字开头
1
2.log
[root@SPR-4 shell_test]# ls |grep  '^[^0-9]'		   #取不是数字开头
ov
pt_1.log
tf.log
[root@SPR-4 shell_test]# ls |grep -v '^[0-9]'          #-v 取反
ov
pt_1.log
tf.log
[root@SPR-4 shell_test]# ls |grep  -v '^[^0-9]'        #取不是数字开头取反
1
2.log
```



## 2. wc

```python
1、 统计当前文件夹下文件的个数
　　ls -l |grep "^-"|wc -l

2、 统计当前文件夹下目录的个数
　　ls -l |grep "^d"|wc -l

3、统计当前文件夹下文件的个数，包括子文件夹里的 
　　ls -lR|grep "^-"|wc -l

4、统计文件夹下目录的个数，包括子文件夹里的
　　ls -lR|grep "^d"|wc -l

grep "^-" 
　　这里将长列表输出信息过滤一部分，只保留一般文件，如果只保留目录就是 ^d

wc -l 
　　统计输出信息的行数，因为已经过滤得只剩一般文件了，所以统计结果就是一般文件信息的行数，又由于一行信息对应一个文件，所以也就是文件的个数。
```

## 3. grep

```
-c   只输出匹配行的计数。
-i   不区分大小写（只适用于单字符）。
-h   查询多文件时不显示文件名。
-l   查询多文件时只输出包含匹配字符的文件名。
-n   显示匹配行及行号。
-v   显示不包含匹配文本的所有行。
-s   不显示不存在或无匹配文本的错误信息。

```

```
root@SPR-4 shell_test]# cat 2.log
Apple
a apple
i eat an apple
you and me , all like apple1

[root@SPR-4 shell_test]# cat 3.log
apple
1  2.log  3.log  4.log  5.log  ov  tf

[root@SPR-4 shell_test]# cat 4.log
aaa
1123
aaa

[root@SPR-4 shell_test]# grep -c 'apple' *.log
2.log:3
3.log:1
4.log:0
5.log:0

[root@SPR-4 shell_test]# grep -ci 'apple' *.log
2.log:4
3.log:1
4.log:0
5.log:0

[root@SPR-4 shell_test]# grep -h 'apple' *.log
a apple
i eat an apple
you and me , all like apple1
apple

[root@SPR-4 shell_test]# grep -l 'apple' *.log
2.log
3.log

[root@SPR-4 shell_test]# grep -n 'apple' *.log
2.log:2:a apple
2.log:3:i eat an apple
2.log:4:you and me , all like apple1
3.log:1:apple

[root@SPR-4 shell_test]# grep -v 'apple' *.log
2.log:Apple
4.log:aaa
4.log:1123
4.log:aaa

[root@SPR-4 shell_test]# ls
1  2.log  3.log  4.log  5.log  ov  tf
[root@SPR-4 shell_test]# ls ov/
ov_1  ov.log  ovov.log

[root@SPR-4 shell_test]# grep 'apple' ov/ov1.log
grep: ov/ov1.log: No such file or directory
[root@SPR-4 shell_test]# grep -s 'apple' ov/ov1.log
[root@SPR-4 shell_test]#

[root@SPR-4 shell_test]# grep 'apple' ov/*
grep: ov/ov_1: Is a directory
grep: ov/ovov.log: Is a directory
[root@SPR-4 shell_test]# grep -s 'apple' ov/*
[root@SPR-4 shell_test]#
```

## 4. sed

```
语法 sed [options] ‘{command}[flags]’ [filename]    
# 中括号内容必有 大括号内容可有可无
sed  # 执行命令
[options]  # 命令选项
{command}[flags]    # sed内部选项和参数
[filename]     # 文件

命令选项
-e script 将脚本中指定的命令添加到处理输入时执行的命令中  多条件，一行中要有多个操作
-f script 将文件中指定的命令添加到处理输入时执行的命令中
-n        抑制自动输出
-i        编辑文件内容
-i.bak    修改时同时创建.bak备份文件。
-r        使用扩展的正则表达式
!         取反 （跟在模式条件后与shell有所区别）

sed常用内部命令
a   在匹配后面添加
i   在匹配前面添加
p   打印
d   删除
s   查找替换
c   更改
y   转换   N D P 

flags
数字             表示新文本替换的模式
g：             表示用新文本替换现有文本的全部实例
p：             表示打印原始的内容
w filename:     将替换的结果写入文件
```

**4.1** **文件内容增加操作，将数据追加到某个位置之后，使用命令a**

```
[root@SPR-1 guojian]# cat data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

#在data1的每行后追加一行新数据内容: append data "haha"
[root@SPR-1 guojian]# sed 'a\append data "haha"' data1
1 the quick brown fox jumps over the lazy dog.
append data "haha"
2 the quick brown fox jumps over the lazy dog.
append data "haha"
3 the quick brown fox jumps over the lazy dog.
append data "haha"
4 the quick brown fox jumps over the lazy dog.
append data "haha"
5 the quick brown fox jumps over the lazy dog.
append data "haha"

[root@SPR-1 guojian]# cat data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

#在data1的每一行追加一行新数据，并保存在data2中
[root@SPR-1 guojian]# sed 'a\append data "haha"' data1 >> data2
[root@SPR-1 guojian]# cat data2
1 the quick brown fox jumps over the lazy dog.
append data "haha"
2 the quick brown fox jumps over the lazy dog.
append data "haha"
3 the quick brown fox jumps over the lazy dog.
append data "haha"
4 the quick brown fox jumps over the lazy dog.
append data "haha"
5 the quick brown fox jumps over the lazy dog.
append data "haha"

#在某一行追加一行数据
[root@SPR-1 guojian]# sed '1a\append data "haha"' data1
1 the quick brown fox jumps over the lazy dog.
append data "haha"
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.
[root@SPR-1 guojian]# sed '2a\append data "haha"' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
append data "haha"
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

#在第二到四行每行后新开一行追加数据: append data "haha"
[root@SPR-1 guojian]# sed '2,4a\append data "haha"' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
append data "haha"
3 the quick brown fox jumps over the lazy dog.
append data "haha"
4 the quick brown fox jumps over the lazy dog.
append data "haha"
5 the quick brown fox jumps over the lazy dog.

#匹配字符串追加: 找到包含"3 the"的行，在其后新开一行追加内容: append data "haha"
[root@SPR-1 guojian]# sed '/3 the/a\append data "haha"' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
append data "haha"
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

//开启匹配模式  /要匹配的字符串/
```

**4.2** **文件内容增加操作，将数据插入到某个位置之前，使用命令 i**

```
#在data1的每行前插入一行新数据内容: insert data "haha"
[root@SPR-1 guojian]# sed 'i\insert data "haha"' data1
insert data "haha"
1 the quick brown fox jumps over the lazy dog.
insert data "haha"
2 the quick brown fox jumps over the lazy dog.
insert data "haha"
3 the quick brown fox jumps over the lazy dog.
insert data "haha"
4 the quick brown fox jumps over the lazy dog.
insert data "haha"
5 the quick brown fox jumps over the lazy dog.

#在第二行前新开一行插入数据: insert data "haha"
[root@HBM-1 guojian]# sed '2i\insert data "haha"' data1
1 the quick brown fox jumps over the lazy dog.
insert data "haha"
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

#在第二到四行每行前新开一行插入数据: insert data "haha"
[root@HBM-1 guojian]# sed '2,4i\insert data "haha"' data1
1 the quick brown fox jumps over the lazy dog.
insert data "haha"
2 the quick brown fox jumps over the lazy dog.
insert data "haha"
3 the quick brown fox jumps over the lazy dog.
insert data "haha"
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

#匹配字符串插入: 找到包含"3 the"的行，在其前新开一行插入内容: insert data "haha"
[root@HBM-1 guojian]# sed '/3 the/i\insert data "haha"' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
insert data "haha"
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

```

**4.3 文件内容修改操作—替换,将一行中匹配的内容替换为新的数据，使用命令s**

```
#从标准输出流中做替换，将test替换为text
[root@HBM-1 guojian]# echo "this is a test" |sed 's/test/text/'
this is a text

#将data1中每行的dog替换为cat
[root@HBM-1 guojian]# sed 's/dog/cat/' data1
1 the quick brown fox jumps over the lazy cat.
2 the quick brown fox jumps over the lazy cat.
3 the quick brown fox jumps over the lazy cat.
4 the quick brown fox jumps over the lazy cat.
5 the quick brown fox jumps over the lazy cat.

#将data1中第二行的dog替换为cat
[root@HBM-1 guojian]# sed '2s/dog/cat/' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy cat.
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

#将data1中第二到第四行的dog替换为cat
[root@HBM-1 guojian]# sed '2,4s/dog/cat/' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy cat.
3 the quick brown fox jumps over the lazy cat.
4 the quick brown fox jumps over the lazy cat.
5 the quick brown fox jumps over the lazy dog.

#匹配字符串替换:将包含字符串"3 the"的行中的dog替换为cat
[root@HBM-1 guojian]# sed '/3 the/s/dog/cat/' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy cat.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

```

**4.4 文件内容修改操作—更改,将一行中匹配的内容替换为新的数据，使用命令c**

```
bash将data1文件中的所有行的内容更改为: change data "data"
[root@SPR-1 guojian]# sed 'c\change data "haha"' data1
change data "haha"
change data "haha"
change data "haha"
change data "haha"
change data "haha"

将data1文件第二行的内容更改为: change data "haha"
[root@SPR-1 guojian]# sed '2c\change data "haha"' data1
1 the quick brown fox jumps over the lazy dog.
change data "haha"
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

将data1文件中的第二、三、四行的内容更改为：change data "haha"
[root@SPR-1 guojian]# sed '2,4c\change data "haha"' data1
1 the quick brown fox jumps over the lazy dog.
change data "haha"
5 the quick brown fox jumps over the lazy dog.

将data1文件中包含"3 the"的行内容更改为: change data "haha"
[root@SPR-1 guojian]# sed '/3 the/c\change data "data"' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
change data "data"
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.
```

4.5 **文件内容修改操作—字符转换，将一行中匹配的内容替换为新的数据，使用命令y。**

```
将data1中的a b c字符转换为对应的 A  B  C字符
[root@SPR-1 guojian]# sed 'y/abc/ABC/' data1
1 the quiCk Brown fox jumps over the lAzy dog.
2 the quiCk Brown fox jumps over the lAzy dog.
3 the quiCk Brown fox jumps over the lAzy dog.
4 the quiCk Brown fox jumps over the lAzy dog.
5 the quiCk Brown fox jumps over the lAzy dog.
```

4.6 **文件内容删除，将文件中的指定数据删除，使用命令d。**

```
删除文件data1中的所有数据
[root@SPR-1 guojian]# sed 'd' data1
[root@SPR-1 guojian]#

删除文件data1中的第三行数据
[root@SPR-1 guojian]# sed '3d' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

删除文件data1第三到第四行的数据
[root@SPR-1 guojian]# sed '3,4d' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

删除文件data1中包含字符串"3 the"的行
[root@SPR-1 guojian]# sed '/3 the/d' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.
```

4.7 **文件内容查看，将文件内容输出到屏幕，使用命令p**

```
打印data1文件内容
[root@SPR-1 guojian]# sed 'p' data1
1 the quick brown fox jumps over the lazy dog.
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

打印data1文件第三行的内容
[root@SPR-1 guojian]# sed '3p' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

打印data1文件第二、三、四行内容
[root@SPR-1 guojian]# sed '2,4p' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.

打印data1文件包含字符串"3 the"的行
(base) [root@SPR-1 guojian]# sed '/3 the/p' data1
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.
可以看得出，打印内容是重复的行，原因是打印了指定文件内容一次，又将读入缓存的所有数据打印了一次，所以会看到这样的效果，
如果不想看到这样的结果，可以加命令选项-n抑制内存输出即可。
[root@SPR-1 guojian]# sed '/3 the/p' -n data1
3 the quick brown fox jumps over the lazy dog.
[root@SPR-1 guojian]# sed '2,4p' -n data1
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
[root@SPR-1 guojian]# sed '3p' -n data1
3 the quick brown fox jumps over the lazy dog.
```

4.8 **在命令行中使用多个命令 -e**

```
将brown替换为green dog替换为cat
[root@SPR-1 guojian]# sed -e 's/brown/green/;s/dog/cat/' data1
1 the quick green fox jumps over the lazy cat.
2 the quick green fox jumps over the lazy cat.
3 the quick green fox jumps over the lazy cat.
4 the quick green fox jumps over the lazy cat.
5 the quick green fox jumps over the lazy cat.
```

4.9 **从文件读取编辑器命令 -f 适用于日常重复执行的场景**

```
1）将命令写入文件
[root@www ~]# vim abc
s/brown/green/
s/dog/cat/
s/fox/elephant/

2）使用-f命令选项调用命令文件
[root@www ~]# sed -f abc data1 
1 the quick green elephant jumps over the lazy cat.
2 the quick green elephant jumps over the lazy cat.
3 the quick green elephant jumps over the lazy cat.
4 the quick green elephant jumps over the lazy cat.
5 the quick green elephant jumps over the lazy cat.
```

4.10 **抑制内存输出 -n**

```
打印data1文件的第二行到最后一行内容  $最后的意思
[root@SPR-1 guojian]# sed -n '2,$p' data1
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.
```

4.11 **使用正则表达式 -r**

```
打印data1中以字符串"3 the"开头的行内容
[root@SPR-1 guojian]# sed -n  -r '/^(3 the)/p' data1
3 the quick brown fox jumps over the lazy dog.
```

**从上述的演示中，大家可以看出，数据处理只是在缓存中完成的，并没有实际修改文件内容，如果需要修改文件内容可以直接使用-i命令选项。在这里我需要说明的是-i是一个不可逆的操作，一旦修改，如果想复原就很困难，几乎不可能，所以建议大家在操作的时候可以备份一下源文件。-i命令选项提供了备份功能，比如参数使用-i.bak，那么在修改源文件的同时会先备份一个以.bak结尾的源文件，然后再进行修改操作。**

4.12 **-i**

```
1）查看文件列表，没有发现data1.bak
(base) [root@SPR-1 test]# ls
data1
2）执行替换命令并修改文件
(base) [root@SPR-1 test]# sed -i.bak 's/brown/green/' data1
3）发现文件夹中多了一个data1.bak文件
(base) [root@SPR-1 test]# ls
data1  data1.bak
4）打印比较一下，发现data1已经被修改，data1.bak是源文件的备份。
[root@SPR-1 test]# cat data1
1 the quick green fox jumps over the lazy dog.
2 the quick green fox jumps over the lazy dog.
3 the quick green fox jumps over the lazy dog.
4 the quick green fox jumps over the lazy dog.
5 the quick green fox jumps over the lazy dog.
[root@SPR-1 test]# cat data1.bak
1 the quick brown fox jumps over the lazy dog.
2 the quick brown fox jumps over the lazy dog.
3 the quick brown fox jumps over the lazy dog.
4 the quick brown fox jumps over the lazy dog.
5 the quick brown fox jumps over the lazy dog.
```

4.13 **标志**

```
演示文档
[root@SPR-1 test]# cat data2
1 the quick brown fox jumps over the lazy dog . dog
2 the quick brown fox jumps over the lazy dog . dog
3 the quick brown fox jumps over the lazy dog . dog
4 the quick brown fox jumps over the lazy dog . dog
5 the quick brown fox jumps over the lazy dog . dog

数字标志：此标志是一个非零正数，默认情况下，执行替换的时候，如果一行中有多个符合的字符串，如果没有标志位定义，那么只会替换第一个字符串，其他的就被忽略掉了，为了能精确替换，可以使用数字位做定义。

替换一行中的第二处dog为cat
[root@SPR-1 test]# sed 's/dog/cat/2' data2
1 the quick brown fox jumps over the lazy dog . cat
2 the quick brown fox jumps over the lazy dog . cat
3 the quick brown fox jumps over the lazy dog . cat
4 the quick brown fox jumps over the lazy dog . cat
5 the quick brown fox jumps over the lazy dog . cat
[root@SPR-1 test]# sed 's/dog/cat/1' data2
1 the quick brown fox jumps over the lazy cat . dog
2 the quick brown fox jumps over the lazy cat . dog
3 the quick brown fox jumps over the lazy cat . dog
4 the quick brown fox jumps over the lazy cat . dog
5 the quick brown fox jumps over the lazy cat . dog

g标志:将一行中的所有符合的字符串全部执行替换
将data1文件中的所有dog替换为cat
[root@SPR-1 test]# sed 's/dog/cat/g' data2
1 the quick brown fox jumps over the lazy cat . cat
2 the quick brown fox jumps over the lazy cat . cat
3 the quick brown fox jumps over the lazy cat . cat
4 the quick brown fox jumps over the lazy cat . cat
5 the quick brown fox jumps over the lazy cat . cat

p标志：打印文本内容，类似于-p命令选项
[root@SPR-1 test]# sed  '3s/dog/cat/p' data2
1 the quick brown fox jumps over the lazy dog . dog
2 the quick brown fox jumps over the lazy dog . dog
3 the quick brown fox jumps over the lazy cat . dog
3 the quick brown fox jumps over the lazy cat . dog
4 the quick brown fox jumps over the lazy dog . dog
5 the quick brown fox jumps over the lazy dog . dog

w filename标志:将修改的内容存入filename文件中
[root@SPR-1 test]# sed  '3s/dog/cat/w text' data2
1 the quick brown fox jumps over the lazy dog . dog
2 the quick brown fox jumps over the lazy dog . dog
3 the quick brown fox jumps over the lazy cat . dog
4 the quick brown fox jumps over the lazy dog . dog
5 the quick brown fox jumps over the lazy dog . dog

[root@SPR-1 test]# ls
data1  data1.bak  data2  text
可以看出，将修改的第三行内容存在了text文件中
[root@SPR-1 test]# cat text
3 the quick brown fox jumps over the lazy cat . dog
[root@SPR-1 test]#

```

4.14 **sed小技巧**

**$= 统计文本有多少行**

```
[root@SPR-1 test]# sed -n '$=' data2
5
[root@SPR-1 test]# sed  '=' data2
1
1 the quick brown fox jumps over the lazy dog . dog
2
2 the quick brown fox jumps over the lazy dog . dog
3
3 the quick brown fox jumps over the lazy dog . dog
4
4 the quick brown fox jumps over the lazy dog . dog
5
5 the quick brown fox jumps over the lazy dog . dog
```

```
输出包含the的所在行的行号（= 用来输出行号）
[root@localhost ~]# sed -n '/the/=' test.txt 

输出以PI开头的行
[root@localhost ~]# sed -n '/^PI/p' test.txt 

输出以数字结尾的行
[root@localhost ~]# sed -n '/[0-9]$/p' test.txt 

输出包含单词wood的行 \< ,\>表示单词边界
[root@localhost ~]# sed -n '/\<wood\>/p' test.txt 
a wood cross!

每行开始添加#字符	
[root@localhost ~]# sed 's/^/#/' test.txt 

在包含the的每行行首添加#字符
[root@localhost ~]# sed '/the/s/^/#/' test.txt 

在每行末尾添加EOF字符
[root@localhost ~]# sed 's/$/EOF/' test.txt 

将3-5行所有的the替换为THE	  	
[root@localhost ~]# sed '3,5s/the/THE/g' test.txt 

将包含the的行中的o替换为O	
    [root@localhost ~]# sed '/the/s/o/O/g' test.txt 

迁移符合条件的文本
H 复制到剪贴板；
g，G 将剪贴板中的数据覆盖/追加到指定行；
w保存为文件；
r读取指定文件；
a 追加指定内容

演示文本
[root@SPR-1 test]# cat test.txt
he was short and fat
the aplle is sweet
so i like go to shopping
the dog is too young
The phone is too old

i don't know
将包含the的行迁移到行尾（；用于多个操作）
H复制到剪贴板---d删除---$G追加到行尾
[root@SPR-1 test]# sed '/the/{H;d};$G' test.txt
he was short and fat
so i like go to shopping
The phone is too old

i don't know

the aplle is sweet
the dog is too young
将1-5行迁移到6行后
[root@SPR-1 test]# sed '1,5{H;d};6G' test.txt


he was short and fat
the aplle is sweet
so i like go to shopping
the dog is too young
The phone is too old
i don't know
将1-5行迁移到7行后
[root@SPR-1 test]# sed '1,5{H;d};7G' test.txt

i don't know

he was short and fat
the aplle is sweet
so i like go to shopping
the dog is too young
The phone is too old
将包含the的行另存为新文件
[root@SPR-1 test]# ls
data1  data1.bak  data2  test.txt  text
[root@SPR-1 test]# sed '/the/w out.file' test.txt
he was short and fat
the aplle is sweet
so i like go to shopping
the dog is too young
The phone is too old

i don't know
[root@SPR-1 test]# ls
data1  data1.bak  data2  out.file  test.txt  text
[root@SPR-1 test]# cat out.file
the aplle is sweet
the dog is too young
在包含the每行后添加文件hostname内容
[root@SPR-1 test]#  sed '/the/r /etc/hostname' test.txt
he was short and fat
the aplle is sweet
SPR-1
so i like go to shopping
the dog is too young
SPR-1
The phone is too old

i don't know
在第3行后插入新行，内容为New
[root@SPR-1 test]# sed '3aNew' test.txt
he was short and fat
the aplle is sweet
so i like go to shopping
New
the dog is too young
The phone is too old

i don't know
在包含the的每行后插入新行		
[root@SPR-1 test]# sed '/the/aNew' test.txt
he was short and fat
the aplle is sweet
New
so i like go to shopping
the dog is too young
New
The phone is too old

i don't know
在第3行后插入多行（\n 换行符）
[root@SPR-1 test]# sed '3aNew1\nNew2' test.txt
he was short and fat
the aplle is sweet
so i like go to shopping
New1
New2
the dog is too young
The phone is too old

i don't know
```



[(206条消息) Shell三剑客之sed命令详解_shell sed_IT.cat的博客-CSDN博客](https://blog.csdn.net/qq_57377057/article/details/126216920)

[(206条消息) sed详解_一道孤独的博客-CSDN博客_sed](https://blog.csdn.net/qq_46470349/article/details/120823482)



## 5. awk

6.eval

7.`` and $()

