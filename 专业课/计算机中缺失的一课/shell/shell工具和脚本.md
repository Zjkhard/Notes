### 基本的bash shell命令

#### 浏览文件系统

1.  ls命令最基本的形式会显示当前目录下的文件和目录：

   ```c++
   [root@zjkStu1 code]# ls
   hello2.txt  hello.txt  ls.txt  mcd.sh  ProfessionalCourse  test  test_four  test_three
   
   
    // 显示隐藏文件
   [root@zjkStu1 code]# ls -a
   .  ..  hello2.txt  hello.txt  ls.txt  mcd.sh  ProfessionalCourse  test  test_four  test_three
   [root@zjkStu1 code]# 
   
   // 显示长列表
    [root@zjkStu1 code]# ls -l
   总用量 24
   -rw-r--r--. 1 root root    6 1月   9 21:28 hello2.txt
   -rw-r--r--. 1 root root   12 1月   9 23:17 hello.txt
   ```

2. 创建文件

   ```c++
   // touch命令创建空文件
   [root@zjkStu1 code]# touch test_one
   [root@zjkStu1 code]# ls -l test_one 
   -rw-r--r--. 1 root root 0 1月  16 21:58 test_one
   [root@zjkStu1 code]# 
   
   // 复制文件
   [root@zjkStu1 code]# cp test_one test_two
   [root@zjkStu1 code]# ls
   hello2.txt  hello.txt  ls.txt  mcd.sh  ProfessionalCourse  test  test_four  test_one  test_three  test_two
   [root@zjkStu1 code]# 
       
   // 重命名文件称为移动（moving）。mv命令可以将文件和目录移动到另一个位置或重新命名
   [root@zjkStu1 code]# mv test_one test_five
   [root@zjkStu1 code]# ls
   hello2.txt  hello.txt  ls.txt  mcd.sh  ProfessionalCourse  test  test_five  test_four  test_three  test_two
   [root@zjkStu1 code]# 
       
   // 删除文件
   [root@zjkStu1 code]# rm -i test_five 
   rm：是否删除普通空文件 "test_five"？y
   [root@zjkStu1 code]# 
   
   // 创建目录
   [root@zjkStu1 code]# mkdir new_dir
   [root@zjkStu1 code]# ls
   hello2.txt  hello.txt  ls.txt  mcd.sh  new_dir  ProfessionalCourse  test  test_four  test_three  test_two
   [root@zjkStu1 code]# 
   
   // 要想同时创建多个目录和子目录，需要加入-p参数：
   mkdir -p New_Dir/Sub_Dir/Under_Dir
   
   // 删除空目录
   [root@zjkStu1 code]# rmdir new_dir/
   [root@zjkStu1 code]# ls
   hello2.txt  hello.txt  ls.txt  mcd.sh  ProfessionalCourse  test  test_four  test_three  test_two
   [root@zjkStu1 code]# 
       
   // 删除非空目录
   [root@zjkStu1 code]# rm -ri My_Dir
       
   // 查看文件内容  -n参数会给所有的行加上行号。 less命令能够识别上下键以及上下翻页键
   [root@zjkStu1 code]# cat hello.txt 
   hello
   hello
   
   // tail命令会显示文件最后几行的内容（文件的“尾部”）。默认情况下，它会显示文件的末尾10行
   [root@zjkStu1 code]# tail -n 1 hello.txt //查看最后一行
   hello
   [root@zjkStu1 code]# 
   
   // head命令，顾名思义，会显示文件开头那些行的内容 默认10行 
   
   ```

#### 监视程序

UID：启动这些进程的用户

PID：进程的进程ID

PPID：父进程的进程号（如果该进程是由另一个进程启动的）。

C：进程生命周期中的CPU利用率。

STIME：进程启动时的系统时间

TTY：进程启动时的终端设备

TIME：运行进程需要的累计CPU时间

CMD：启动的程序名称

```
[root@zjkStu1 ~]# ps -l
F S   UID    PID   PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
4 S     0  13753  13749  0  80   0 - 29196 do_wai pts/2    00:00:00 bash
0 R     0  13805  13753  0  80   0 - 38309 -      pts/2    00:00:00 ps
```



##### 杀死进程

kill  3940



##### 查看挂载磁盘空间

df

du  命令可以显示某个特定目录（默认情况下是当前目录）的磁盘使用情况



##### 搜索数据

grep

```c++
[root@zjkStu1 code]# grep as hello.txt  //搜索as
fasdf
[root@zjkStu1 code]# 
    
//如果要指定多个匹配模式，可用-e参数来指定每个模式。
[root@zjkStu1 code]# grep -e s -e a hello.txt 
edfs
dsf
ad
sda
sda

// grep加正则表达式
[root@zjkStu1 code]# grep [sd] hello.txt 
edfs
dsf
ad
sda
sda
```

查看环境变量

env

设置局部环境变量

echo $my_variable

删除局部变量

unset my_variable

改变权限

chmod 



#### 脚本

第一个简单脚本

```shell
#!/bin/bash
# This Script display the date and who's logged on
echo -n  "The time and date sre: "
date
echo "let's see who's logged into the system "
who
```

运行

```
[root@zjkStu1 shell]# ./firstScript 
The time and date sre: 2022年 01月 19日 星期三 22:25:23 CST
let's see who's logged into the system 
root     :0           2022-01-16 16:15 (:0)
root     pts/0        2022-01-16 17:01 (:0)
root     pts/1        2022-01-16 17:26 (:0)
root     pts/2        2022-01-19 22:06 (192.168.184.1)
root     pts/3        2022-01-19 19:20 (192.168.184.1)
[root@zjkStu1 shell]#
```

变量

环境变量

```shell
#!/bin/bash
# display user information from the system.
echo "User info for userid: $USER"
echo UID: $UID
echo HOME: $HOME
```

用户变量

```shell

#!/bin/bash
# testing variable
days=10
guest="Katie"
echo "$guest checked in $days days ago"
days=5
guest="Jessica"
echo "$guest checked in $days days ago"

```

命令替换

```shell
#!/bin/bash
testing=$(date)
echo "The date and time are: " $testing
```

重定向输入输出

输出

```shell
$ date >> test6
$ cat test6
user pts/0 Feb 10 17:55
Thu Feb 10 18:02:14 EDT 2014
$
```

输入

```shell
$ wc < test6
```

管道

执行算术运算