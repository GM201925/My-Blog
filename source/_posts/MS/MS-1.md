---
title: Lecture 01 Shell
date: 2026-03-29 20:03:04
categories: Missing Semester
---
使用Windows的同学请先安装WSL或Linux虚拟机

**Argument Parsing**
参数间以空格分隔，第一个参数是命令，例如
```shell
❯ echo hello      world
hello world
❯ echo "hello       world"
hello       world
```

cd: 切换当前目录
```shell
❯ cd ~
❯ cd Fuyukawa/../././
# 还是在~，上面意味着先切换到（当前目录下的）Fuyukawa文件夹，
# 之后再cd ..之后再cd .等
```

一个命令就是一个program，当你输入一个命令时，shell会对$PATH变量中记录的路径逐个寻找，直到找到这个命令
```shell
❯ which date
/usr/bin/date
❯ /usr/bin/date
Sun Mar 29 20:29:07 CST 2026
❯ date
Sun Mar 29 20:29:08 CST 2026

```
>which  returns  the  pathnames of the files (or links) which would be executed in the current environment, had its arguments been given as commands in a strictly POSIX-conformant shell.  
It does this by searching the PATH for executable files matching the names of the arguments. It does not canonicalize path names.

grep 在文件中筛选
```shell
❯ cat test.txt
hello
world
你好
code
test.txt
test1
test2
test3
❯ grep "w" test.txt
world

❯ grep -r h .
./w2.txt:what
./w1.txt:hello
❯ find . -name "*.txt" -exec grep -l what {} \;
./w2.txt
# 在当前文件夹下找以.txt结尾的文件
# -exec是将找到的路径放入{}中并执行\;之前的命令
# 这里是执行grep -l what w1.txt
# 以及grep -l what w2.txt
# -l是打印出含有what的文件的路径
```

**awk**
**sed**

管道符 **|** 把左侧命令的输出作为右侧命令的输入