---
title: HPC-Intro
date: 2026-06-01 15:33:42
tags:
---
准备HPC的lab时的一些琐碎的知识，记录一下

写信时，信封会写上收件地址和收件人，如果把网络世界比作城市，网卡就像是城市中的建筑，MAC地址就是建筑的物理地址，ip地址就是住在建筑里的人

在计算机网络中发送信息，我们只需要填写目标IP和内容，操作系统会通过查询arp表获取对方的MAC地址，从网卡发出

设备的通信都是靠网卡进行的，每张网卡出厂时都会写入一个MAC地址。一个计算机通过网线连到交换机的某个端口，交换机将端口号和MAC地址绑定。
同时，网卡还需要配置IP地址。当电脑插上网线或手机连入到wifi，操作系统网络协议栈会自动向外发送一包DHCP请求，路由器收到请求后会为其分配一个IP地址，并通过DHCP回复报文发送回去。操作系统收到回包后，将其分配的IP地址配置到网卡里。一个网络中，IP地址是唯一的。

操作系统如何知道对方的MAC地址呢？这一步由arp协议完成。例如计算机a向计算机b发送消息，操作系统会先发送一包arp广播报文出去，问一下某个IP地址的MAC地址是多少，除了该IP地址的设备，都会丢弃这包请求报文，只有那个IP地址的设备会回复自己的MAC地址是多少，计算机a知道后会将MAC地址缓存以便下次使用，然后补齐信息（MAC+IP+内容）从网卡发送出去。

交换机根据数据包中的目标MAC地址找到计算机b的端口，发送给计算机b

什么是交换机？两台电脑通过网线传输数据，如果有五台电脑，那每个电脑就要接4根网线，现在将电脑都接入交换机，根据我们上面说的进行数据传输。
这些设备和它们连接的交换机组成了一个局域网，将两个局域网的交换机连接起来，得到更大的一个局域网，交换机之间又通过交换机连接……

互联网服务提供商（如移动等）就负责将各地设备接入到一个大的网络中

## IPV4、NAT和IPV6
IPV4由32个位组成，每8位二进制数为一组，转换为十进制，如``192.168.0.1``
IPV4是不够用的，我们很难做到让每个设备拥有自己的独立IP，我们引入新技术NAT(网络地址转换)
假设5台电脑ABCDE接入一个路由器，它们的IP为192.168.0.1到182.168.0.5，它们接入的路由器接入广域网的IP为6.6.6.6，路由器作为网关（简单说就是局域网内部设备与外部广域网交互的出口），假设电脑A要发送信息给广域网中IP为8.8.8.8的设备，网关会把发送过来的数据的IP映射成6.6.6.6，这样，五台设备在广域网中就共用了一个IP，即6.6.6.6，但这样就无法区分五台设备了。

网关以不同端口与外部交互，把端口映射给局域网内的各个设备，传输数据时除了IP地址映射，还有端口号映射，比如A就映射成``6.6.6.6 : 1000``，要注意的是，这里的1000并不是什么路由器上的物理端口，可以理解成一个标识

这五台设备的IP就是私有IP，而6.6.6.6就是公网IP，8.8.8.8设备下也完全可以有五台设备，它们的IP地址是192.168.0.1到182.168.0.5

IPV6由8组四位十六进制数组合而成，正在慢慢取代IPV4


总结一下，arp协议只在局域网内起作用，通过IP地址找到同一网络内设备的MAC地址，如果要跨网络，要把信息交给网关，例如上面提到的A电脑发给8.8.8.8，arp这时就会问网关的MAC地址，发送出去的信息包含网关的MAC地址和最终目的地IP地址(8.8.8.8).
交换机看到目标MAC是网关，就根据MAC地址表，从连接网关那个物理口送出去。
网关路由器：收到后拆开，看到目标IP是 8.8.8.8，就开始用路由表在互联网上转发。同时执行NAT，把源IP从 192.168.0.1 换成公网IP 6.6.6.6

## Linux-Managing Users
```bash
sudo useradd username
```

```bash
cat /etc/passwd
```
我们可以通过这个文件了解系统都有哪些用户，每一行是类似
``roland:x:1000:1000:Roland,,,:/home/roland:/bin/bash``
的输出，第一个1000是uid，一般小于1000的uid是系统用户，使用之前的命令添加一个普通用户账户

删除用户
```bash
sudo userdel username
```

添加用户时，我们希望把它添加到home目录里，即拥有一个home/username
```bash
sudo useradd -m username
```
删除用户，同时删除它的home directory
```bash
sudo userdel -r username
```

新添加的用户没有密码
```bash
passwd #修改当前用户的密码

sudo passwd username #修改其他用户的密码
```

**创建系统用户**
系统用户用于在后台运行任务，无法登录
```bash
sudo useradd -r username
```

**Etsy Password File**
``roland:x:1000:1000:Roland,,,:/home/roland:/bin/bash``
第一列是用户名，第二列x代表加密的密码，第三列是uid，第四列是gid，即组id，下一列是用户信息，然后是主目录(home directory)，``/bin/bash``代表的是该用户登录时使用的shell

```bash
sudo cat /etc/shadow | grep roland

roland:$y$j9T$AAy/ae/XdhQYM5RvDk5hd/$rPiM7PLECfXDItKhdsrsjTjPdRSrt7jKwbR6CkVKBE.:20604:0:99999:7:::
```
第二列是加密的密码，第二列是上次更改密码的时间（自1970.1.1算起过了多少天），第三列是距离下次可以更改密码还有多少天，第四列是还有多少天密码过期，第五列是距离密码过期还剩多少天时会在shell提醒

## File Permissions
linux系统的文件或文件夹的权限，有owner的权限，即这个文件属于谁，有group的权限，即这个文件属于哪个组，还有其他权限，即所有其他用户都有的权限

```bash
drwxrwxr-x 3 roland roland 4096 May 31 06:17 test
-rw-rw-r-- 1 roland roland    6 May 31 06:19 text_1.txt
```
d代表文件夹，如果是文件，则是-
第一个rwx代表owner的权限，可以读，可以写，可以运行(对文件是运行，对文件夹是能列出文件夹内容)
第二个rwx代表group，即组里的用户都有rwx权限
其他所有用户可以read，execute

```bash
sudo chown newowner filename
sudo chown newowner:newgroup filename

```
r=4,w=2,x=1
```bash
sudo chmod 774 text_1.txt

-rwxrwxr-- 1 roland roland    6 May 31 06:19 text_1.txt
```

## Environment Variables
In Linux and Unix-based systems, environment variables are dynamic named values stored within the system that are used by applications launched in shells or subshells. In simple terms, an environment variable is a variable with a name and an associated value.


|Task|Command|
|:-:|:-:|
|List all environment variables|``printenv`` or ``env``|
|Print a specific variable|``printenv HOME`` or ``echo $HOME``|
|Set a shell variable|``MY_VAR="value"``|
|Set an environment variable|``export MY_VAR="value"``|
|Remove a variable|``unset MY_VAR``|
|Add to PATH|``export PATH="$HOME/bin:$PATH"``|
|List all variables and functions|``set``|
|Make variable persistent|Add it to your shell startup file such as ``~/.bashrc``, ``~/.zshrc``, or ``~/.profile``|
|Reload config after changes|``source ~/.bashrc`` or ``source ~/.zshrc``|

**Environment Variables and Shell Variables**
变量的格式
```
KEY=value
KEY="Some other value"
KEY=value1:value2
```
- The names of the variables are case-sensitive. By convention, environment variables should have UPPER CASE names.
- When assigning multiple values to the variable, they must be separated by the colon : character.
- There is no space around the equals = symbol.

Variables can be classified into two main categories: environment variables and shell variables.

**Environment variables** are variables that are available system-wide and are inherited by all spawned child processes and shells.

**Shell variables** are variables that apply only to the current shell instance. Each shell such as zsh and bash, has its own set of internal shell variables.

``env`` – The command allows you to run another program in a custom environment without modifying the current one. When used without an argument it will print a list of the current environment variables.
``printenv`` – The command prints all or the specified environment variables.
``set`` – The command sets or unsets shell variables. When used without an argument it will print a list of all variables including environment and shell variables, and shell functions.
``unset`` – The command deletes shell and environment variables.
``export`` – The command sets environment variables.


``printenv``能展示环境变量，如果带参数只会展示那个变量的值
```bash
roland@debian:~$ printenv HOME
/home/roland
```

**Setting Environment Variables**
创建一个shell variable
```bash
MY_VAR='Hello'
```
但这并不是环境变量
```bash
roland@debian:~$ MY_VAR='Hello'
roland@debian:~$ printenv MY_VAR
roland@debian:~$ export MY_VAR
roland@debian:~$ printenv MY_VAR
Hello

# 也可以
export MY_NEW_VAR="My New Var"

unset MY_VAR # 移除环境/shell变量
```

**PATH**
PATH - A list of directories to be searched when executing commands. When you run a command the system will search those directories in this order and use the first found executable.
```bash
echo $PATH

export PATH="$HOME/bin:$PATH"
# This prepends $HOME/bin to the beginning of PATH, so executables in that directory will take priority.

export PATH="$PATH:/opt/myapp/bin"
# To append a directory to the end of PATH
```

**Persistent Environment Variables**
想让变量在你每次登录时都自动生效，就需要把 export 命令写入 Shell 的启动配置文件中。

按作用范围划分：

所有用户（系统级）：

/etc/environment：存放简单键值对，不支持 export 命令语法，由 PAM 读取。

/etc/profile：在登录 Shell 时执行，需要用 export。

/etc/profile.d/*.sh：存放独立脚本，由 /etc/profile 调用。

当前用户（用户级）：

~/.bashrc：最常用，适用于交互式非登录 Shell（如图形界面打开的终端）。

~/.profile 或 ~/.bash_profile：适用于登录 Shell（如通过 SSH 登录），通常设置全局环境变量
```bash
source ~/.bashrc   # 重新加载 ~/.bashrc 文件
```