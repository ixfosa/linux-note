## 前置知识

### Shell概述

Shell 既是一种脚本编程语言，也是一个连接内核和用户的软件。

但Shell 本身并不是内核的一部分，它只是站在内核的基础上编写的一个应用程序

然而 Shell 也有着它的特殊性，就是开机立马启动，并呈现在用户面前；用户通过 Shell 来使用 Linux，不启动 Shell 的话，用户就没办法使用 Linux。



1. 接收并处理用户输入的命令（调用内核函数）
2. 连接其它程序
3. 支持编程
4. 一种脚本语言（解释型：一边执行一边翻译，弱类型）



### Shell和GUI用户接口

1. `GUI` 一种图形化的用户接口，易于操作。

2. `Shell` 实际上是一个能够解释和分析用户键盘输入，执行输入中的命令，然后返回结果的一个解释程序（例如在 `linux` 下比较常用的 `Bash`），可以通过下面的命令查看当前的 `Shell` ：

   ```shell
   [root@ixfosa ~]# echo $SHELL
   /bin/bash
   
   
   [root@ixfosa ~]# ll /bin/bash
   -rwxr-xr-x. 1 root root 960472 Aug  3  2017 /bin/bash
   ```



### Shell种类

1. sh：Bourne shell，由 AT&T 公司的 Steve Bourne开发，为了纪念他，就用他的名字命名了。
2. csh：由柏克莱大学的 Bill Joy 设计的，这个 shell 的语法有点类似C语言，所以才得名为 C shell 

3. tcsh：tcsh 是 csh 的增强版，加入了命令补全功能，提供了更加强大的语法支持。
4. ash：一个简单的轻量级的 Shell，占用资源少，适合运行于低内存环境，但是与 bash shell 完全兼容。

5. `bash`：bash shell 是 Linux 的·默认 shell



查看 Shell

```shell
[root@ixfosa ~]# cat /etc/shells 
/bin/sh
/bin/bash
/sbin/nologin
/usr/bin/sh
/usr/bin/bash
/usr/sbin/nologin
```

>  Linux 上，sh 已经被 bash 代替，`/bin/sh`往往是指向`/bin/bash`的符号链接。



### 进入Shell的]方式

1. 进入 Linux 控制台

   一种进入 Shell 的方法是让 Linux 系统退出图形界面模式，进入控制台模式。

   现代 Linux 系统在启动时会自动创建几个虚拟控制台（Virtual Console），其中一个供图形桌面程序使用，其他的保留原生控制台的样子。虚拟控制台其实就是 Linux 系统内存中运行的虚拟终端（Virtual Terminal）。

   从图形界面模式进入控制台模式也很简单，往往按下`Ctrl + Alt + Fn(n=1,2,3,4,5...)`快捷键就能够来回切换。

2. 使用终端

   进入 Shell 的另外一种方法是使用 Linux 桌面环境中的终端模拟包（Terminal emulation package），也就是终端（Terminal），这样在图形桌面中就可以使用 Shell。

   > CentOS 默认的图形界面程序是 GNOME，该终端模拟包也是 GNOME 自带的。
   >
   > GNOME 终端，Linux 还有其他的终端模拟包，例如：
   >
   > + xterm 终端
   > + Konsole 终端: KDE 桌面的终端模拟包，Konsole 整合了基本的 xterm 特性以及一些更高级的类似 Windows 应用程序的特性。



### Shell命令的本质

Shell 命令分为两种：

- 内置命令：Shell 自带的命令，它在 Shell 内部可以通过`函数`来实现，当 Shell 启动后，这些命令所对应的代码（函数体代码）也被加载到内存中，所以使用内置命令是非常快速的。
-  外部命令：一个命令就对应一个应用程序。运行外部命令要开启一个新的进程，所以效率上比内置命令差很多。

> 用户输入一个命令后，Shell 先检测该命令是不是内置命令，如果是就执行，如果不是就检测有没有对应的外部程序：有的话就转而执行外部程序，执行结束后再回到 Shell；没有的话就报错，告诉用户该命令不存在。



区分内置命令和外部命令

```shell
[root@ixfosa ~]# type cd
cd is a shell builtin

[root@ixfosa ~]# type ls
ls is aliased to `ls --color=auto'
```



外部命令检索路径：

```shell
[root@ixfosa ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
```

> 不同的路径之间以`:`分隔。



总结

Shell 内置命令的本质是一个自带的函数，执行内置命令就是调用这个自带的函数。因为函数代码在 Shell 启动时已经被加载到内存了，所以内置命令的执行速度很快。

Shell 外部命令的本质是一个应用程序，执行外部命令就是启动一个新的应用程序。因为要创建新的进程并加载应用程序的代码，所以外部命令的执行速度很慢。



### Shell命令的选项和参数

Shell 命令附带参数：

+ `cd dir`：入当前目录下的 dir 目录，其中dir命令的参数。
+ `echo "hello"`命令表示输出字符串并换行，其中`"hello"`就是 echo 命令的参数。



Shell 命令附带选项：

+ `ls -l`命令用来显示当前目录下的所有文件以及它们的详细信息，其中`-l`就是 ls 命令的选项。
+ `echo -n "hello"`表示在输出字符串后不换行，其中`-n`是 echo 命令的选项，`"hello"`是 echo 命令的参数。



有些命令的选项后面也可以附带参数：

- `read -n 1 sex`命令用来读取一个字符并赋值给 sex 变量，其中`-n`是 read 命令的选项，`1`是`-n`选项的参数，`sex`是 read 命令的参数。



> 不管是内置命令还是外部命令，它后面附带的数据最终都以参数的形式传递给了函数。



### hell命令提示符

提示符格式(CentOS )

```shell
[root@ixfosa ~]# 
```

各个部分的含义如下：

- `[]`：是提示符的分隔符号，没有特殊含义。

- `root`：表示当前登录的用户

- `@`：是分隔符号，没有特殊含义。

- `ixfosa`：表示当前系统的简写主机名。

- `~`代表用户当前所在的目录为主目录（home 目录）。

  如果用户当前位于主目录下的 bin 目录中，那么这里显示的就是`bin`。

- `#`是命令提示符。

  Linux 用这个符号标识登录的用户权限等级：

  + root 用户，`#`；
  + 普通用户，`$`。



第二层命令提示符

有些命令不能在一行内输入完成，需要换行，这个时候就会看到第二层命令提示符。第二层命令提示符默认为`>`

```shell
[root@ixfosa ~]# echo "hello shell"
hello shell
[root@ixfosa ~]# echo "
> hello linux..."     # 直到遇见第二个 "。

hello linux...
```

> 提示符`>`用来告诉用户命令还没输入完成，请继续输入。



### 修改Shell命令提示符

Shell 通过 `PS1 ` 和 `PS2` 这两个环境变量来控制提示符的格式

- `PS1` 控制最外层的命令提示符格式。
- `PS2 `控制第二层的命令提示符格式。

```shell
[root@ixfosa ~]# echo $PS1
[\u@\h \W]\$

[root@ixfosa ~]# echo $PS2
>

[root@ixfosa ~]# PS1="[\t][\u]\$ "
[12:51:43][root]# PS1="[ixfosa]\$ "
[ixfosa]# 
```

| 字符 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| \a   | 铃声字符                                                     |
| \d   | 格式为“日 月 年”的日期                                       |
| \e   | ASCII 转义字符                                               |
| \h   | 本地主机名                                                   |
| \H   | 完全合格的限定域主机名                                       |
| \j   | shell 当前管理的作业数                                       |
| \1   | shell 终端设备名的基本名称                                   |
| \n   | ASCII 换行字符                                               |
| \r   | ASCII 回车                                                   |
| \s   | shell 的名称                                                 |
| \t   | 格式为“小时:分钟:秒”的24小时制的当前时间                     |
| \T   | 格式为“小时:分钟:秒”的12小时制的当前时间                     |
| \@   | 格式为 am/pm 的12小时制的当前时间                            |
| \u   | 当前用户的用户名                                             |
| \v   | bash shell 的版本                                            |
| \V   | bash shell 的发布级别                                        |
| \w   | 当前工作目录                                                 |
| \W   | 当前工作目录的基本名称                                       |
| \!   | 该命令的 bash shell 历史数                                   |
| \#   | 该命令的命令数量                                             |
| \$   | 如果是普通用户，则为美元符号`$`；如果超级用户（root 用户），则为井号`#`。 |
| \nnn | 对应于八进制值 nnn 的字符                                    |
| \\   | 斜杠                                                         |
| \[   | 控制码序列的开头                                             |
| \]   | 控制码序列的结尾                                             |



### 第一个Shell脚本

```shell
[root@ixfosa ~]# mkdir script
[root@ixfosa ~]# cd script/

[root@ixfosa script]# vi hello.sh
```

> `hello.sh`：扩展名sh代表 shell，扩展名并不影响脚本执行

hello.sh 中的内容：

```shell
1 #!/bin/bash         
2 # Author: ixfosa
3
4 echo "hello shell..."

解释：

第一行：#! 约定的标记，告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell；后面的`/bin/bash`就是指明了解释器的具体位置。

第二行：# 后面跟着注释信息

第四行：echo 命令用于向标准输出文件（Standard Output，stdout，一般就是指显示器）输出文本。在 .sh 文件中使用命令与在终端直接输入命令的效果是一样的。
```



### 执行Shell脚本

运行 Shell 脚本：

+ 在新进程中运行
+ 在当前 Shell 进程中运行



1. 在新进程中运行 Shell 脚本
   1.  将 Shell 脚本作为程序运行

      Shell 脚本也是一种解释执行的程序，可以在终端直接调用，注意：需要使用 chmod 命令给 Shell 脚本加上执行权限

      ```shell
      # 赋予 执行 权限
      [root@ixfosa script]# chmod +x hello.sh 
      
      # ./表示当前目录，整条命令的意思是执行当前目录下的 hello.sh 脚本。
      # 如果不写 ./ ，Linux 会到系统路径（由 PATH 环境变量指定）下查找 hello.sh，而系统路径下显然不存在这个脚本，所以会执行失败。
      [root@ixfosa script]# ./hello.sh 
      hello shell...
      ```

      

   2. 将 Shell 脚本作为参数传递给 Bash 解释器

      直接运行 Bash 解释器，将脚本文件的名字作为参数传递给 Bash

      ```shell
      [root@ixfosa script]# /bin/bash hello.sh 
      hello shell...
      
      # bash 是一个外部命令
      [root@ixfosa script]# bash hello.sh 
      hello shell...
      ```

      

   > 通过这种方式运行脚本，不需要在脚本文件的第一行指定解释器信息



2. 检测是否开启了新进程

   Linux 中的每一个进程都有一个唯一的 ID，称为 `PID`，使用`$$`变量就可以获取当前进程的 PID。`$$`是 Shell 中的特殊变量

   ```shell
   [root@ixfosa script]# cat check.sh
   #!/bin/bash
   
   echo $$ #输出当前进程PID
   
   
   [root@ixfosa script]# echo $$
   1666
   
   [root@ixfosa script]# chmod +x check.sh 
   
   [root@ixfosa script]# ./check.sh 
   1745
   
   [root@ixfosa script]# /bin/bash check.sh 
   1746
   
   [root@ixfosa script]# bash check.sh 
   1747
   ```

   

3. 在当前进程中运行 Shell 脚本

   `source`是 Shell 内置命令，它会读取脚本文件中的代码，并依次执行所有语句。

   source 命令的用法为：`source filename`

   > 也可以简写为：`. filename`
   >
   > 注意：点号`.`和文件名中间有一个空格。

   

   1. 使用 source 运行 helo.sh

      ```shell
      [root@ixfosa script]# source hello.sh 
      hello shell...
      [root@ixfosa script]# . hello.sh
      hello shell...
      ```

   2. 检测是否在当前 Shell 进程中

      ```shell
      [root@ixfosa script]# echo $$
      1666
      
      [root@ixfosa script]# source check.sh 
      1666
      
      [root@ixfosa script]# . check.sh 
      1666
      ```

      

### Shell四种运行(启动)方式



1. Shell 一共有四种运行方式：

   + 交互式的登录 Shell
   + 交互式的非登录 Shell
   + 非交互式的登录 Shell
   + 非交互式的非登录 Shell

   > 登录式：输入用户名和密码后再使用 Shell
   >
   > 非登录式：直接使用 Shell

   > 交互式：Shell 中一个个地输入命令并及时查看它们的输出结果，整个过程都在跟 Shell 不停地互动
   >
   > 非交互式：运行一个 Shell 脚本文件，让所有命令批量化、一次性地执行



2. 判断 Shell 是否是交互式

   1. 查看变量`-`的值，如果值中包含了字母`i`，则表示交互式（interactive）。

      ```shell
      # 包含了i，为交互式
      
      # xshell中 或 虚拟终端（Ctrl+Alt+Fn） 直接输出
      [root@ixfosa script]# echo $-
      himBH
      
      [root@ixfosa script]# . shellRunStyle.sh 
      himBH
      
      
      # 
      --------------------------------
      
      # 编写 shell 脚本 输出
      [root@ixfosa script]# cat shellRunStyle.sh 
      #!/bin/bash
      
      echo $-
      
      # 不包含i，为非交互式。注意，必须在新进程中运行 Shell 脚本。
      [root@ixfosa script]# bash shellRunStyle.sh 
      hB
      ```

   2. 查看变量`PS1`的值，如果非空，则为交互式，否则为非交互式，因为非交互式会清空该变量。

      ```shell
      # 非空，为交互式。
      [root@ixfosa script]# echo $PS1
      [\u@\h \W]\$
      
      [root@ixfosa script]# . shellRunStyle2.sh 
      [\u@\h \W]\$
      
      
      -----------------------------
      
      [root@ixfosa script]# cat shellRunStyle2.sh 
      #!/bin/bash
      echo $PS1
      
      # 空值，为非交互式。注意，必须在新进程中运行 Shell 脚本。
      [root@ixfosa script]# bash shellRunStyle2.sh 
      
      
      ```

      



3. 判断 Shell 是否为登录式

   执行`shopt login_shell` 判断其值：

   + `on`：表示为登录式，
   + `off`：为非登录式。

   ```shell
   # CentOS GNOME 桌面环境
   [root@ixfosa script]# shopt login_shell
   login_shell    	off
   
   
   # 虚拟终端 或 Xshell 中
   [root@ixfosa script]# shopt login_shell
   login_shell    	on
   ```



4. 同时判断交互式、登录式

   要同时判断是否为交互式和登录式，可以简单使用如下的命令：

   ```shell
   echo $PS1; shopt login_shell
   
   或者
   
   echo $-; shopt login_shell
   ```

   

5. 常见的 Shell 启动方式

   1. 通过 Linux 控制台（不是桌面环境自带的终端）或者 ssh 登录 Shell 时（这才是正常登录方式），为交互式的登录 Shell。

      ```shell
      ]$ echo $PS1;shopt login_shell
      [\u@\h \W]\$
      login_shell    on
      ```

      

   2. 执行 bash 命令时默认是非登录的，增加`--login`选项（简写为`-l`）后变成登录式。

      ```shell
      [root@ixfosa script]# cat test.sh
      #!/bin/bash
      
      echo $-; shopt login_shell
      
      [root@ixfosa script]# bash -l ./test.sh
      hB
      login_shell    on
      ```

      

   3. 使用由`()`包围的[组命令或者命令替换进入子 Shell 时，子 Shell 会继承父 Shell 的交互和登录属性。

      ```shell
      [root@ixfosa script]# bash
      [root@ixfosa script]# (echo $PS1;shopt login_shell)
      [\u@\h \W]\$
      login_shell    off
      
      [root@ixfosa script]# bash -l
      [root@ixfosa script]# (echo $PS1;shopt login_shell)
      [\u@\h \W]\$
      login_shell    on
      ```

   4. ssh 执行远程命令，但不登录时，为非交互非登录式。

      ```shell
      [root@ixfosa script]# ssh localhost 'echo $PS1;shopt login_shell'
      
      login_shell     off
      ```

   5.  在 Linux 桌面环境)下打开终端时，为交互式的非登录 Shell。



### Shell配置文件的加载

无论是否是交互式，是否是登录式，Bash Shell 在启动时总要配置其运行环境，例如初始化环境变量、设置命令提示符、指定系统命令路径等。这个过程是通过加载一系列配置文件完成的，这些配置文件其实就是 Shell 脚本文件。



1. bash的配置文件：

   + `profile`类：为交互式登录的shell进程提供配置

     + 全局：对所有用户都生效；
       `etc/profile `
       `/etc/profile.d/*.sh`
     + 用户个人：仅对当前用户有效；
       `~/.bash_profile`

     + 功用：

       1. 用于定义环境变量；
       2. 运行命令或脚本；

       

   + `bashrc`类：为非交互式登录的shell进程提供配置

     + 全局：
       `/etc/bashrc `
     + 用户个人：
       `~/.bashrc`
     + 功用：
       1. 定义本地变量；
       2. 定义命令别名；

   > 注意：仅管理员可修改全局配置文件；
   >
   > `~`表示用户主目录。
   >
   > `*`是通配符



2. Shell配置文件的加载顺序

   + 交互式登录shell进程：

     ```shell
     /etc/profile --> 
     	/etc/profile.d/* --> 
     		~/.bash_profile --> 
                 ~/.bashrc --> 
                 	/etc/bashrc
     ```

   

   + 非交互式登录shell进程：

     ```shell
     ~/.bashrc --> /etc/bashrc --> /etc/profile.d/*
     ```

   > `~/.bashrc` 才是最重要的文件，因为不管是否登录都会加载该文件。

   

> 命令行中定义的特性，例如变量和别名作用域为当前shell进程的生命周期；
> 配置文件定义的特性，只对随后新启动的shell进程有效；



让通过配置文件定义的特性立即生效：

1. 通过命令行重复定义一次；
2. 让shell进程重读配置文件；
   + ~]# source  /PATH/FROM/CONF_FILE
   + ~]# .   /PATH/FROM/CONF_FILE



## 变量