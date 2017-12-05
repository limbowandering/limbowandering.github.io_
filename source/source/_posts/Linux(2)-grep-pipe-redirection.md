---
title: Linux过滤器,管道与重定向
date: 2017-10-03 23:59:00
categories: Linux
tags: Linux
---

##  Linux过滤器

过滤器是什么? 过滤器就是从标准输入按行读取内容, 并且把内容写到标准输出, 这是最简单的没有任何过滤功能的过滤器, 当然了也有各种功能的过滤器. 

### 基本行过滤器

> 面向行的过滤器

#### cat

`cat [-nbs] file`

cat可以把一个至多个文件连接在一起, 然后输出到屏幕. 

-n number 在每行输出添加行号.

-b blank 和-n一起使用时, 空白行不编号

-s squeeze 将多个空白行替换为一个

#### tac

tac是cat的反向拼写, 所以功能刚好是相反的, 再把读到的内容写入到标准输出时把行的顺序翻转过来, 当然了也可以是多个文件, 这个在看日志文件的时候很方便. 

#### Split

把文件分割开来

根据行数, split默认分割行数是1000行.

`split [-d][-a num][-l lines][file [prefix]]` 

-d : 表示分割的文件名从0开始编号

-a : 表示使用-d数字的位数

-l : 表示分割的行数

prefix: 表示分割文件名字的前缀.

#### rev

> rev reverse 反转每一行字符的顺序, 跟字符串反转是一样的, 不过按行处理

#### head

`head [-n lines]`

head从标准输入读取数据, 输出数据的前n行默认显示前十行

#### tail

` tail [-n lines]`

从标准输入读取数据, 输出数据的后n行, 默认显示后十行

#### less

`less files` 

默认显示一个屏幕的数据

###  列过滤器

#### colrm

> colrm  = column remove

`colrm startcol endcol`

`colrm 1 2 < t ` 删除第一列和第二列

#### cut

> 从标准输入中选择列作为标准输出

`cut [-cd list] file`

-c (column列) 是跟colrm一样按列选择, 可以使用"," 组成list

-d(delimiter 分隔符) 根据特殊符号分割行, 然后使用-f (field 域)选择分割的列

` cut -c 1-2,3-5 t`

`cut -f 1-d ";" t` 

### 比较过滤器

#### cmp

`cmp file1 file2` 比较两个文件按照字节, 所以文件可以是二进制或者文本.

#### diff

`diff [-iwbc] file1 file2`

-i: 忽略大小写这个好像在很多地方都用到了

-w: 忽略所有的空白符(ab,a b是一样的

-b: 忽略空白符, 不过是把多个空白符合并成一个

-c: 输出不同的时候, 显示上下文比较容易理解



### Linux管道

"|"是管道命令操作符, 简称管道符. 利用Linux所提供的管道符|将两个命令隔开, 管道符左边命令的输出就会作为管道符右边命令的输入. 连续使用管道意味着第一个命令的输出会作为第二个命令的输入, 第二个命令的输出又会作为第三个命令的输入. 以此类推.

它仅能处理经由前面一个指令传出的正确输出信息, 也就是标准输出的信息, 对于standard error信息没有直接处理能力.

用法示例: ls -l | more

该命令列出当前目录中的文档,并把输出送给more命令作为输入, more命令分页显示文件列表.

### Linux中重定向

#### 重定向符号

`>` 输出重定向到一个文件或者设备 覆盖原来的文件

`>!` 输出重定向到一个文件或设备 强制覆盖原来的文件

`>>` 输出重定向到一个文件或设备 追加原来的文件

`<` 输出重定向到一个程序

#### 为何要使用命令输出重定向

- 当屏幕输出的信息很重要, 而且我们需要将他存下来的时候.
- 后台执行中的程序, 不希望他干扰屏幕正常的输出结果时
- 一些系统的例行命令(例如写在/etc/crontab中的文件) 的执行结果, 希望他可以存下来时.
- 错误讯息与正确信息需要分别输出时候.


