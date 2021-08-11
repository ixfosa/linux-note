## history-命令历史

命令历史：shell进程会其会话中保存此前用户提交执行过的命令；

1. 管理命令历史；
   ​登录shell时，会读取命令历史文件中记录下的命令：`~/.bash_history`(/home/USERNAME，/root)
   ​登录进shell后新执行的命令只会记录在缓存中；这些命令会用户退出时“追加”至命令历史文件中；

2. 定制history的功能，可通过环境变量实现：

   + `HISTSIZE`：shell进程可保留的命令历史的条数；

   + `HISTFILE`：持久保存命令历史的文件；

   + `HISTFILESIZE`：命令历史文件的大小；

     > 修改变量的值：`NAME='VALUE'`

   ```shell
   [ixfosa@ixfosa ~]$ echo $HISTFILE
   /home/ixfosa/.bash_history
   [ixfosa@ixfosa ~]$ echo $HISTSIZE
   1000
   [ixfosa@ixfosa ~]$ echo $HISTFILESIZE
   1000
   ```

3. 命令用法：

   + history [-c] [-d 偏移量] [n] 
   + history -anrw [文件名] 
   + history -ps 参数 [参数...]

4. 常用选项：

   | 选项        | 描述                                                         |
   | ----------- | ------------------------------------------------------------ |
   | `-a`        | 将当前shell会话的历史命令追加到命令历史文件中,命令历史文件是保存历史命令的配置文件 |
   | `-c`        | 清空当前历史命令列表                                         |
   | `-d`        | 删除指定命令历史                                             |
   | -n          | 从命令历史文件中读取本次Shell会话开始时没有读取的历史命令    |
   | `-r`        | 读取命令历史文件到当前的Shell历史命令内存缓冲区              |
   | `-w`        | 把当前的shell历史命令内存缓冲区的内容写入命令历史文件        |
   | `history n` | 显示最近的 n 条命令；                                        |



5. 调用上一条命令的最后一个参数：

   + `!$`: 
   + `ESC, .`
   + `Alt+.`

   

6. 调用命令历史列表中的命令：
   + `!n`：再一次执行历史列表中的第 n 条命令；
   + `!!`：再一次执行上一条命令；
   + `!STRING`：再一次执行命令历史列表中最近一个以STRING开头的命令；

>  注意：命令的重复执行有时候需要依赖于幂等性；
>
>  幂等性：提交一次或多次，结果都是一样的。



7. 控制命令历史的记录方式：
   环境变量：`HISTCONTROL`
   + `ignoredups`：忽略重复的命令；连续且相同方为“重复”；
   + `ignorespace`：忽略所有以空白开头的命令；
   + `ignoreboth`：ignoredups, ignorespace；

```shell
[root@ixfosa home]# echo $HISTCONTROL
ignoredups
```

8. 修改环境变量值的方式：

​	`export 变量名="值"`
​	变量赋值：把赋值符号后面的数据存储于变量名指向内存空间；

```shell
[root@ixfosa ~]# export HISTCONTROL="ignorespace"   // 双引号
[root@ixfosa ~]# echo $HISTCONTROL
ignorespace
```



## tab-命令补全

shell程序在接收到用户执行命令的请求，分析完成之后，最左侧的字符串会被当作命令；

命令查找机制：
		查找内部命令；
		根据PATH环境变量中设定的目录，自左而右逐个搜索目录下的文件名；

直接补全：`Tab`，用户给定的字符串只有一条唯一对应的命令；
				以用户给定的字符串为开头对应的命令不唯一，则再次Tab会给出列表；



## tab-路径补全

在给定的起始路径下，以对应路径下的打头字串来逐一匹配起始路径下的每个文件：
tab：
		如果能惟一标识，则直接补全；
		否则，再一次tab，给出列表；



## $?-命令的执行结果状态

1. 程序执行有两类结果：

   + 程序的返回值；
   + 程序的执行状态结果；

 2. bash使用特殊变量 `$?` 保存最近一条命令的执行状态结果：

    + `0`：成功

    + `1-255`：失败

      ```shell
      [root@ixfosa ~]# pwd
      /root
      [root@ixfosa ~]# echo $?
      0
      [root@ixfosa ~]# lls
      -bash: lls: command not found
      [root@ixfosa ~]# echo $?
      127
      ```

3. 引用命令的执行结果：

   + $(COMMAND)

   + \`COMMAN\`

     ```shell
     [root@ixfosa tmp]# mkdir $(date "+%Y-%m")
     [root@ixfosa tmp]# ls -l
     drwxr-xr-x. 2 root root   4096 May  7 11:39 2021-05
     ...
     ```



## 引用

1. 强引用：' '    (单引号)
2. 弱引用：" "（双引号）
3. 命令引用：``（反引号）



## 快捷键

| 快捷键 | 描述                       |
| ------ | -------------------------- |
| Ctrl+a | 跳转至命令行行首           |
| Ctrl+e | 跳转至命令行行尾           |
| Ctrl+u | 删除光标`左边`的所有字符； |
| Ctrl+k | 删除光标`右边`的所有字符； |
| Ctrl+l | 清屏，相当于clear          |

​	

## {}-命令行展开

1. `~` 自动展开为用户的家目录，或指定的用户的家目录；

   + `~USERNAME`：展开为指定用户的主目录

     ```shell
     [root@ixfosa ~]# cd ~ixfosa
     [root@ixfosa ixfosa]# cd ~
     [root@ixfosa ~]#
     ```

2. `{}`：可承载一个以逗号分隔的列表，并将其展开为多个路径

   + 例如：`/tmp/{a,b}` = `/tmp/a`,  ` /tmp/b`



3. 问题1：如何创建/tmp/x/y1, /tmp/x/y2, /tmp/x/y1/a, /tmp/x/y1/b？

   ```shell
   mkdir -pv /tmp/x/{y1/{a,b},y2}
   ```

4. 问题2：如何创建a_c, a_d, b_c, b_d；

   ```shell
   mkdir -pv {a, b}_{c, d}
   ```



## globbing-文件名通配

匹配模式：元字符

+ `*`：匹配任意长度的任意字符

+ `?`：匹配任意单个字符

+ `[]`：匹配指定范围内的任意单个字符

  有几种特殊格式：

  + [a-z], [A-Z], [0-9], [a-z0-9]
  + `[[:upper:]]`：所有大写字母
  + `[[:lower:]]`：所有小写字母
  + `[[:alpha:]]`：所有字母
  + `[[:digit:]]`：所有数字
  + `[[:alnum:]]`：所有的字母和数字
  + `[[:space:]]`：所有空白字符
  + `[[:punct:]]`：所有标点符号

+ `[^]`：匹配指定范围外的任意单个字符

  如：

  + `[^[:upper:]]`
  + `[^0-9]`
  + ``[^[:alnum:]]`

  

1. 练习1：显示/var目录下所有以l开头，以一个小写字母结尾，且中间出现一位任意字符的文件或目录；

   ```shell
   [root@ixfosa ~]# ls -d /var/l?[[:lower:]]
   /var/lib  /var/log
   ```

   

2. 练习2：显示 /etc 目录下，以任意一位数字开头，且以非数字结尾的文件或目录；

   ```shell
   [root@ixfosa ~]# ls -d /etc/[[:digit:]]*[^[:digit:]]
   ls: cannot access /etc/[[:digit:]]*[^[:digit:]]: No such file or directory
   [root@ixfosa ~]# ls -d /etc/[0-9]*[^0-9]
   ```

   ​		

3. 练习3：显示 /etc 目录下，以非字母开头，后面跟一个字母及其它任意长度任意字符的文件或目录；

   ```
   [root@ixfosa ~]# ls -d /etc/[^[:alpha:]][[:alpha:]]*
   ```

   ​		

4. 练习4：复制 /etc 目录下，所有以m开头，以非数字结尾的文件或目录至 /tmp/test 目录；

   ```shell
   [root@ixfosa ~]# mkdir /tmp/test
   [root@ixfosa ~]# cp -r /etc/m*[^0-9] /tmp/test/
   ```

   

5. 练习5：复制/usr/share/man目录下，所有以man开头，后跟一个数字结尾的文件或目录至 /tmp/man/ 目录下；

   ```shell
   [root@ixfosa ~]# mkdir /tmp/man 
   [root@ixfosa ~]# cp -r /usr/share/man/man[0-9] /tmp/man
   ```

   ​	

6. 练习6：复制/etc目录下，所有以.conf结尾，且以m,n,r,p开头的文件或目录至 /tmp/conf.d/ 目录下；

   ```shell
   [root@ixfosa ~]# mkdir /tmp/conf.d
   [root@ixfosa ~]# cp -r /etc/[mnrp]*.conf /tmp/conf.d/
   [root@ixfosa ~]# ls /tmp/conf.d/
   man_db.conf  mke2fs.conf  nsswitch.conf  resolv.conf  rsyslog.conf
   ```

   ​	

## 命令hash

`hash` ：命令负责显示与清除命令运行时系统优先查询的哈希表（hash table）。

hash命令：

+ hash：列出
+ hash -d COMMAND：删除
+ hash -r：清空

```shell
[root@ixfosa ~]# hash
hits	command
   4	/usr/bin/install
   1	/usr/bin/cat
   1	/usr/bin/touch
   5	/usr/bin/mktemp
   1	/usr/bin/mkdir
  11	/usr/bin/ls

[root@ixfosa ~]# hash -d install
[root@ixfosa ~]# hash
hits	command
   1	/usr/bin/cat
   1	/usr/bin/touch
   5	/usr/bin/mktemp
   1	/usr/bin/mkdir
  11	/usr/bin/ls

  
[root@ixfosa ~]# hash -r
[root@ixfosa ~]# hash
hash: hash table empty
```



## 多命令执行

```shell
~]# COMMAND1; COMMAND2; COMMAND3; ...
```

逻辑运算：

运算数：真(true, yes, on, 1)
			   假(false, no, off, 0)

+ 与：
  	1 && 1 = 1
     				1 && 0 = 0
     				0 && 1 = 0
     				0 && 0 = 0
+ 或：
  	1 || 1 = 1
     				1 || 0 = 1
     				0 || 1 = 1
     				0 || 0 = 0
+ 非：
  	! 1 = 0
     				! 0 = 1



短路法则：

+ ~]# COMMAND1 && COMMAND2
  + COMMAND1为“假”，则COMMAND2不会再执行；
  + 否则，COMMAND1为“真”，则COMMAND2必须执行；

+ ~]# COMMAND1 || COMMAND2
  + COMMAND1为“真”，则COMMAND2不会再执行；
  + 否则，COMMAND1为“假”，则COMMAND2必须执行；

