1. 管道：连接程序，实现将前一个命令的输出直接定向后一个程序当作输入数据流

   语法格式：COMMAND1 | COMMAND2 | COMMAND3 | ...



2. `tee` 命令：读取标准输入的数据，tee指令会从标准输入设备读取数据，将其内容输出到标准输出设备，同时保存成文件 。

   语法格式：COMMAND | tee  /PATH/TO/SOMEFILE

   ```shell
   [root@ixfosa ~]# ls test/ | tee ./haha
   dir
   dir2
   [root@ixfosa ~]# ls
   anaconda-ks.cfg  haha  test
   [root@ixfosa ~]# cat haha 
   dir
   dir2
   ```

   使用指令”`tee`”将用户输入的数据同时保存到文件”file1″和”file2″中，输入如下命令：

   ```shell
   
   [root@ixfosa ~]# tee file1 file2
   Hello Linux
   Hello Linux
   
   [root@ixfosa ~]# cat file1
   Hello Linux
   
   [root@ixfosa ~]# cat file2
   Hello Linux
   ```


