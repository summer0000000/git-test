# Liunx笔记

## 1 基础操作

配置ip：/etc/sysconfig/network-scripts/xxxens

重启网卡  

1. nmcli c reload                       *# 重新加载配置文件*
2. nmcli c up ens32                     *# 重启ens32网卡*

man [命令]来查看各个命令的使用文档，

### 1.1 常见命令

> - ls（英文全拼：list files）: 列出目录及文件名
> - cd（英文全拼：change directory）：切换目录
> - pwd（英文全拼：print work directory）：显示目前的目录
> - mkdir（英文全拼：make directory）：创建一个新的目录
> - rmdir（英文全拼：remove directory）：删除一个空的目录
> - cp（英文全拼：copy file）: 复制文件或目录
> - rm（英文全拼：remove）: 删除文件或目录
> - mv（英文全拼：move file）: 移动文件与目录，或修改文件与目录的名称

+ **su -root** 切换到root用户
+ **cat  text.txt** 查看文件内容
  + cat -n 文件名 显示所有行的行号，包括空行

+ **echo  xxxx(内容)  >  text.txt(文件)**       写入内容到文件中
+ **cd**     切换目录
  + cd ~  切换到/root
  + cd -   切换到上一目录

+ **ls** 展示文件
  + -a ：全部的文件，连同隐藏文件( 开头为 . 的文件) 一起列出来(常用)
  + -d ：仅列出目录本身，而不是列出目录内的文件数据(常用)
  + -l ：长数据串列出，包含文件的属性与权限等等数据；(常用)
  + -t  按照修改时间排序
  + -r  逆序展示


当为 **d** 则是目录

当为 **-** 则是文件；

若是 **l** 则表示为链接文档(link file)；

若是 **b** 则表示为装置文件里面的可供储存的接口设备(可随机存取装置)；

若是 **c** 则表示为装置文件里面的串行端口设备，例如键盘、鼠标(一次性读取装置)

+ ln（link）创建链接文件   默认创建的是一个硬文件   
  + **软链接**： 类似Window下的快捷方式，删除的源文件，快捷方式也访问不了！
  + 硬文件 ：与源文件指向的 i结点 相同 ，一个内容变化其他都变化
  + ln -s  创建一个软文件   类似于windows中的快捷方式
  



查看任务进程：top
查看内存 free
查看操作历史 history

文件查看命令 cat / tail
tail 显示指定文件的末尾10行
tail -f 显示最新追加类容
tail -c x [文件名] 显示最后几个字符



**history 查看已经执行过历史命令**

```
# 查看历史命令
history
# 情况历史命令
history -c
```

**时间日期类**

1. 基本语法

date [option] + [format]

date -s 日期时间 设置日期时间

date + “日期时间格式” 指定显示时使用的日期



### 1.2 文件文档的CRUD

+ 文件权限

> `-rwxrw-r-- `
>
> 可以划分为4部分    `- `   、`rwx `   、`rw- `、` r--`
>
> 其中rwx代表：可读、可写、可执行
>
> 分别代表：文件类型、文件所有者的权限（user）、同组用户的权限（group）、其他用户权限（other）
>
> #### 更改文件所有者
>
> ```
> chown [–R] 所有者 文件名
> chown [-R] 所有者:属组名 文件名
> ```
>
> 案例：
> ```
> [root@www ~] cd ~
> [root@www ~]# chown bin install.log
> [root@www ~]# ls -l
> -rw-r--r--  1 bin  users 68495 Jun 25 08:53 install.log
> --------------------------------------------------------------
> [root@www ~]# chown root:root install.log
> [root@www ~]# ls -l
> -rw-r--r--  1 root root 68495 Jun 25 08:53 install.log
> ```
>
> 
>
> #### 修改文件权限
>
> chmod（change mode） 修改文件权限： + 代表添加权限 - 代表删除权限
>
> 案例：
>
> + chmod  u+x   给所有者添加可执行的权限 
> + chmod  ug+x   给同组用户添加可执行的权限 
> + chmod  u+x   给其他用户添加可执行的权限 
>
> **转化为数字来添加权限**
>
> **r 对应4**  **w代表2 x代表1**  **rwx代表6**
>
> chmod 777 

+ **mkdir [-mp] 目录名称** 创建新目录
  + -m ：配置文件的权限喔！直接配置，不需要看默认权限 (umask) 的脸色～
  + -p ：帮助你直接将所需要的目录(包含上一级目录)递归创建起来！
  
+ **rmdir [-p] 目录名称** 删除空的目录
  + **-p ：**从该目录起，一次删除多级空目录
  
+ **touch  text.txt** 添加文档
  + touch 命令原来是修改时间的，当没有文件的时候会自动创建文件
  
+ **rm [-fir] 文件或目录** 移除文件或目录
  + -f ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；
  + -i ：互动模式，在删除前会询问使用者是否动作
  + -r ：递归删除啊！最常用在目录的删除了！这是非常危险的选项！！！
  
+ **mv [-fiu] source destination**  代表移动文件和目录 同时可以修改文件名字
  
  + -f ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
  
  + -i ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！
  
  + -u ：若目标文件已经存在，且 source 比较新，才会升级 (update)
  
  + ```
    mv oldNameFile newNameFile 重命名
    mv /tmp/moveFile /targerFolder 移动文件
    ```
  
+ **cp [-adfilprsu] 来源档(source) 目标档(destination)**     复制命令
  
  + **-a：**相当於 -pdr 的意思，至於 pdr 请参考下列说明；(常用)
  + **-d：**若来源档为链接档的属性(link file)，则复制链接档属性而非文件本身；
  + **-f：**为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；
  + **-i：**若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)
  + **-l：**进行硬式链接(hard link)的链接档创建，而非复制文件本身；
  + **-p：**连同文件的属性一起复制过去，而非使用默认属性(备份常用)；
  + **-r：**递归持续复制，用於目录的复制行为；(常用)
  + **-s：**复制成为符号链接档 (symbolic link)，亦即『捷径』文件；
  + **-u：**若 destination 比 source 旧才升级 destination ！
  
+ **cat [-AbEnTv]** 由第一行开始显示文件内容
  + -A ：相当於 -vET 的整合选项，可列出一些特殊字符而不是空白而已；
  + -b ：列出行号，仅针对非空白行做行号显示，空白行不标行号！
  + -E ：将结尾的断行字节 $ 显示出来；
  + -n ：列印出行号，连同空白行也会有行号，与 -b 的选项不同；
  + -T ：将 [tab] 按键以 ^I 显示出来；
  + -v ：列出一些看不出来的特殊字符
  
+ **tac** 从最后一行开始显示，可以看出 tac 是 cat 的倒着写！

+ **nl**  显示的时候，顺道输出行号！**（常用）**

+ **more**   一页一页的显示文件内容

  + more 一页一页的显示文件内容，带余下内容的（空格代表翻页，enter 代表向下看一行， :f 行号）

+ less 与 more 类似，但是比 more 更好的是，他可以往前翻页！

  + 空格下翻页，pageDown，pageUp键代表翻动页面！退出 q 命令，查找字符串 /要查询的字符向下查询，向上查询使用？要查询的字符串，n 继续搜寻下一个，N 上寻找！

+ **head [-n number] 文件**  只看头几行
  + -n ：后面接数字，代表显示几行的意思

+ **tail [-n number] 文件**  只看尾巴几行
  + -n ：后面接数字，代表显示几行的意思
  + -f ：表示持续侦测后面所接的档名，要等到按下[ctrl]-c才会结束tail的侦测

### 1.3 关机系统

sync 将数据由内存同步到硬盘中

shutdown -c 取消前一个关机命令
shutdown -h 时间 多久关机
shutdown -h now 马上关机 （需要root用户）
shutdown -r 重启

halt  停机，关闭系统，但不断电   -n / -w / -d / -i / -p   

poweroff  关机，断电

init 0 关机
init 6 重启
init 3 切换字符界面
init 5 切换到可视化界面

reboot 重启命令（需要root用户） 等同于 shutdown-r now

### 1.4 磁盘管理

- **df [-ahikHTm] [目录或文件名]**（英文全称：disk free）：列出文件系统的整体磁盘使用量
  - `-h`：以人类可读的方式显示输出结果（例如，使用 KB、MB、GB 等单位）。
  - `-T`：显示文件系统的类型。
  - `-t <文件系统类型>`：只显示指定类型的文件系统。
  - `-i`：显示 inode 使用情况。
  - `-H`：该参数是 `-h` 的变体，但是使用 1000 字节作为基本单位而不是 1024 字节。这意味着它会以 SI（国际单位制）单位（例如 MB、GB）而不是二进制单位（例如 MiB、GiB）来显示磁盘使用情况。
  - `-k`：这个选项会以 KB 作为单位显示磁盘空间使用情况。
  - `-a`：该参数将显示所有的文件系统，包括虚拟文件系统，例如 `proc`、`sysfs` 等。如果没有使用该选项，默认情况下，`df` 命令不会显示虚拟文件系统。
- **du [-ahskm] 文件或目录名称**（英文全称：disk used）：检查磁盘空间使用量

  - -a ：列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。
  - -h ：以人们较易读的容量格式 (G/M) 显示；
  - -s ：仅显示指定目录或文件的总大小，而不显示其子目录的大小。
  - -S ：包括子目录下的总计，与 -s 有点差别。
  - -k ：以 KBytes 列出容量显示；
  - -m ：以 MBytes 列出容量显示；
- **fdisk [-l] 装置名称**：用于磁盘分区

  - -l ：输出后面接的装置所有的分区内容。若仅有 fdisk -l 时， 则系统将会把整个系统内能够搜寻到的装置的分区均列出来。
- **mkfs [-t 文件系统格式] 装置文件名** 磁盘格式化

  - -t ：可以接文件系统格式，例如 ext3, ext2, vfat 等(系统有支持才会生效)
- mount 挂载
- 卸载：umount -f [挂载位置] 强制卸载



## 2 vim文本编辑命令

操作界面有三种模式：命令模式（command mode）、插入模式（Insert mode）和底行模式（last line mode） 

####  命令模式

- 刚进入文件就是命令模式，通过方向键控制光标位置，

- 使用命令"dd"删除当前整行 
- 使用命令“yy”进行复制 命令“p”进行粘贴
-  -使用命令"**/字段**"进行查找  例如 /hello 查找hello  或者是 /vhello
  - **”/字段\c“ 可以区分大小写 ** **通过n/N进行向下向下查找**
  - **?word** 向光标之上寻找一个字符串名称为 word 的字符串
  - 在底行模式下输入**:set ic** 来忽略大小写
- **i** -- 切换到输入模式，在光标当前位置开始输入文本。
- **x** -- 删除当前光标所在处的字符。
- **:** -- 切换到底线命令模式，以在最底一行输入命令。
- **a** -- 进入插入模式，在光标下一个位置开始输入文本。
- **o**：在当前行的下方插入一个新行，并进入插入模式。
- **O** -- 在当前行的上方插入一个新行，并进入插入模式。
- **dd** -- 剪切当前行。
- **yy** -- 复制当前行。
- **p**（小写） -- 粘贴剪贴板内容到光标下方。
- **P**（大写）-- 粘贴剪贴板内容到光标上方。
- **u** -- 撤销上一次操作。
- **Ctrl + r** -- 重做上一次撤销的操作。
-  **.**    这是小数点，表示重复前一个操作 （常用）
- **:w** -- 保存文件。
- **:q** -- 退出 Vim 编辑器。
- **:q!** -- 强制退出Vim 编辑器，不保存修改。
- **功能键[Home]或 0** 游标移动到这一行的第一个字元处 **功能键[End]或 $** 游标移动到这一行的最后一个字元处 （常用）
- **H  移动到屏幕最上方那一行的第一个字元       M 移动到屏幕中央那一行的第一个字元       L 移动到这个屏幕最下方那一行的第一个字元       G 移动到这个档案的最后一行** （常用） 
- **nG n** 为数字。游标移动到这个档案的第 n 行 gg 游标移动到这个档案的第一行 （常用） 
- **数字+[Enter]** n 为数字。游标向下移动 n 行 （常用）
-  **[Del] 或 x** 向后删除一个字元 （常用） **X** 向前删除一个字元 （常用）
- **nx n** 为数字，连续向后删除 n 个字符。举例来说，我要连续删除 10 个字符， 『10x』。
- **dd** 删除游标所在的那一行 （常用） **ndd** n为数字，删除游标所在的那一行往下数 的 n 个行，（常用）<br/> **d1G 删除游标所在的那行到第一行的所有数据 dG 删除游标所在的那行到最后一行的所有数据 **

#### 插入模式 

- 按"i"在光标所在字符前开始插入  按"a"在光标所在字符后开始插入  按"o"在光标所在行的下面另起一新行插入  
- 按"ESC"进入底行模式  底行模式  按"："进入底行模式  

- 此时可以对文件内容进行编辑，左下角会显示 "-- 插入 --""  
- **字符按键以及Shift组合**，输入字符
- **ENTER**，回车键，换行
- **BACK SPACE**，退格键，删除光标前一个字符
- **DEL**，删除键，删除光标后一个字符
- **方向键**，在文本中移动光标
- **HOME**/**END**，移动光标到行首/行尾
- **Page Up**/**Page Down**，上/下翻页
- **Insert**，切换光标为输入/替换模式，光标将变成竖线/下划线
- **ESC**，退出输入模式，切换到命令模式

#### 底行模式

+ 退出编辑   :q     强制退出   :q!   保存并退出   :wq 

-   vim +10 filename.txt      打开文件并跳到第10行  

-   vim -R /etc/passwd     以只读模式打开文件

-   替换**:s**

  - :s/hello/sss  将该行中的首个hello替换为sss    :s/hello/sss/g  为整行替换
  - 1,5s/hello/sss 将第一到第五行的hello替换为ssss
  - 1,$s/hello/sss 将第一行到全局末尾中的hello替换为sss

-   搜索：v

-   撤回按键   u   反撤回 ctrl + r

  | Ctrl] + [f]      | 屏幕『向下』移动一页，相当于 [Page Down]按键 (常用) |
  | ---------------- | --------------------------------------------------- |
  | [Ctrl] + [b]     | 屏幕『向上』移动一页，相当于 [Page Up] 按键 (常用)  |
  | [Ctrl] + [d]     | 屏幕『向下』移动半页                                |
  | [Ctrl] + [u]     | 屏幕『向上』移动半页                                |
  | 0 或功能键[Home] | 这是数字『 0 』：移动到这一行的最前面字符处 (常用)  |
  | $ 或功能键[End]  | 移动到这一行的最后面字符处(常用)                    |

#### vi 的配置文件 

**.vimrc** 配置用于每次打开都设置好了

## 3.账号管理

```
创建用户 useradd xx   useradd -g [组] [用户名] 给密码 passwd xx
查看系统用户 cat /etc/passwd
查看用户组 cat /etc/group
修改用户组属性 usermod -G [组] [用户名]
删除用户 userdel -f 强制删除 -r 删除相关文件
```

### 3.1 添加用户useradd

useradd -选项 用户名

-m： 自动创建这个用户的主目录 /home/qinjiang

-G : 给用户分配组！

```
[root@aubin test]# useradd -m cjm1
理解一下本质：Linux中一切皆文件，这里的添加用户说白了就是往某一个文件中写入用户的信息了！
/etc/passwd
```

### 3.2 删除用户 userdel

userdel -r qinjiang 删除用户的时候将他的目录页一并删掉！

```
[root@aubin test]# userdel -r cjm1
```

### 3.3 修改用户 usermod

```
[root@kuangshen home]# usermod -d /home/233 qinjiang
修改完毕之后查看配置文件即可！
```

### 3.4  切换用户

1.切换用户的命令为：su username 【username是你的用户名】

2.从普通用户切换到root用户，还可以使用命令：sudo su

3.在终端输入 exit 或 logout 或使用快捷方式ctrl+d，可以退回到原来用户，其实ctrl+d也是执行的exit命令

```
[root@aubin test]# su aubin
[aubin@aubin test]$ exit
exit
[root@aubin test]# 

```

##  4.用户组管理

+ **groupadd**  创建用户组
+ **groupdel** 删除用户组
+  **groupmod -g -n** **修改用户组的权限信息和名字**
+ newgrp root 切换用户组

## 5.进程管理

+ ps -xx ：查看当前系统中正在执行的各种进程的信息！
  + -a 显示当前终端运行的所有的进程信息（当前的进程一个） 
  + -u 以用户的信息显示进程 
  + -x 显示后台运行进程的参数！

```
ps -aux 查看所有的进程
ps -aux|grep mysql
| 在Linux这个叫做管道符 A|B
grep 查找文件中符合条件的字符串！
ps -xx|grep 进程名字
```

+ ps -ef：可以查看到父进程的信息

  ```
  ps -ef|grep mysql # 看父进程我们一般可以通过目录树结构来查看！
  进程树！
  pstree -pu
  -p 显示父id
  -u 显示用户组
  ```

+ kill -9 进程的id  结束进程：杀掉进程，等价于window结束任务！

## 6.系统服务管理   

### 6.1基本语法

 systemctl start | stop | restart | status 服务名

 查看服务的方法：`ls - al /usr/lib/systemd/system`

查看服务状态 systemctl status <服务名>
关闭服务  systemctl stop 《服务名》 
打开  systemctl start 《服务名》
重启                    restart
添加开机启动       enable
静止 开机启动	disable
查看服务               list-unit-files

## 7.帮助命令

man [命令或配置文件] 获取帮助信息

```
man -f cd
cd (1)               - bash built-in commands, see bash(1)
cd (n)               - Change working directory
[root@lys ~]# man 1 cd # 1为上面的括号显示
```

## 8.搜索查找类

### 8.1 find 查找文件或目录

find [搜索范围] [选项]

| 选项           | 功能                                                         |
| -------------- | ------------------------------------------------------------ |
| -name 查询方式 | 按照指定文件名查找模式查找文件                               |
| -user 用户名   | 查找属于指定用户名所有文件                                   |
| -size 文件大小 | 按照指定文件大小查找文件，单位为 b-块 （512字节） c- 字节 w-字 2字节 k - 千字节 M -兆字节 G-吉字节 |

```
# 按名称查找
find -name nginx.conf
# 按路径加名称查找
find /root -name nginx.conf
# 按照后缀查找
# "*"匹配多个字段,"?"匹配单个字符
find -name "*.txt"
# 按用户查找
find -user lys
# 按大小查找 +大于 -小于
find -size +1M
find -size -1M
```

### 8.2 locate 快速定位文件路径

 locate 搜索文件

 由于locate指令基于数据库进行查询，所以第一此运行前，必须使用updatedb指令创建locate数据库

```
updatedb
locate nginx.conf
# 查看指令所在位置
which ls
whereis locate
```

### 8.3   grep过滤查找及"|"管道符

管道符，|，表示将前一个命令的处理结果输出传递给后面的命令处理

grep 选项 查找内容 源文件

- -n 显示匹配行及行号
-  -v ：取反
-  -q ：静默，常用于判断
- -R：可以查看目录下的内容
- -o：只找到这个关键字就可以\
-  -B2：查看文件的前两行
- -A2：查看文件的后两行
- C2：查看文件上下两行

```
# 显示location在nginx.conf的哪几行
grep -n location nginx.conf 
# 查找某文件在该目录的第一个
ls | grep -n test
```

```
wc 查看单词数量
wc nginx.conf
136  302 3022 nginx.conf
行数  单词数量 字节数
```

## 9.SSH配置与使用：安全的远程访问方式

1.安装和启动SSH服务

```
# 示例代码：安装OpenSSH服务器
sudo apt-get install openssh-server    # Ubuntu/Debian
sudo yum install openssh-server        # CentOS/RHEL
```

```
# 示例代码：启动SSH服务
sudo systemctl start ssh
sudo systemctl enable ssh
```

2.配置SSH服务

```
# 在配置文件中，可以设置SSH服务监听的端口、允许的用户、禁止root登录等。
# 示例代码：编辑sshd_config文件
sudo nano /etc/ssh/sshd_config
```

重启SSH服务

```
# 示例代码：重启SSH服务使配置生效
sudo systemctl restart ssh
```

生成SSH密钥对 同git一样

```
# 示例代码：生成SSH密钥对
ssh-keygen -t rsa -b 4096 -f ~/.ssh/my_key
# 示例代码：手动复制公钥到目标服务器
cat ~/.ssh/my_key.pub | ssh user@remote_server 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
# 直接连接
ssh 用户名@服务器地址 
ssh root@192.168.40.130
```

配置SSH客户端

```
# 示例代码：编辑SSH客户端配置文件
nano ~/.ssh/config
# 示例代码：为远程主机配置别名
Host my_server
    HostName remote_server
    User user
    Port 2222
    IdentityFile ~/.ssh/my_key
```

使用SSH传输文件  、 使用SSH隧道

```
# 示例代码：使用SCP传输文件到远程主机 - 要从本地机器复制一个文件到远程机器，请使用下面的语法
scp [source file] [user]@[destination host]:[destination path]
scp file.txt john@192.168.1.100:~/
scp /path/to/local/file user@remote_server:/path/to/remote/directory
# 示例代码：建立SSH隧道
ssh -L 8080:localhost:80 user@remote_server

# 从远程机器复制一个文件到本地机器
scp [user]@[source host]:[source path] [destination file]
# IP地址为 “192.168.1.100 “的远程机器上复制一个名为 “file.txt “的文件，并将其保存在本地机器的主目录下
scp john@192.168.1.100:~/file.txt ~
```

SFTP（安全文件传输协议）

```
# 使用 SFTP 传输文件，你首先需要使用以下命令在远程机器上打开一个SFTP会话—-
sftp [user]@[host]
# 要与IP地址为 “192.168.1.100 “的远程机器和用户 “john “打开一个 SFTP 会话
sftp john@192.168.1.100

put [源文件] [目标文件] – 从本地机器上传文件到远程机器
get [源文件] [目标文件] – 从远程机器下载一个文件到本地机器
ls – 列出远程机器上当前目录下的文件
lls – 列出本地机器上当前目录下的文件
cd [directory] – 改变远程机器上的当前目录
lcd [directory] – 改变本地机器上的当前目录
```

# shell笔记

编写第一个shell脚本

**#!** 是一个约定的标记,  脚本以#!/bin/bash开头（指定解析器）

```
#!/bin/bash 
echo "Hello World !"
```

运行shell脚本的方法（两种）

```
1.作为执行程序 
chmod +x ./test.sh  #使脚本具有执行权限
./test.sh  #执行脚本
2.作为解析器参数
/bin/sh test.sh
/bin/bash test.sh
```

## shell变量

### 1.系统变量

常用系统变量: `$HOME、$PWD、$SHELL、$USER`等

查看系统变量的值： echo $HOME

显示当前Shell中所有变量：set

### 2.自定义变量(重点)

#### 2.1 基本语法

（1）定义变量：变量=值
（2）撤销变量：**unset** 变量
（3）声明静态变量：**readonly**变量，注意：不能unset,不能修改

#### 2.2 变量定义规则

（1）变量名称可以由字母、数字和下划线组成，但是不能以数字开头，环境变量名建议大写。
（2）等号两侧不能有空格
（3）在bash中，变量默认类型都是字符串类型，无法直接进行数值运算。
（4）变量的值如果有空格，需要使用双引号或单引号括起来。

案例：

在bash中，变量默认类型都是字符串类型，无法直接进行数值运算

```
[root@hadoop102 ~]$ C=1+2
[root@hadoop102 ~]$ echo $C
1+2
```

变量的值如果有空格，需要使用双引号或单引号括起来

```
[root@hadoop102 ~]$ D=I love banzhang
-bash: world: command not found
[root@hadoop102 ~]$ D="I love banzhang"
[root@hadoop102 ~]$ echo $A
I love banzhang
```

可把变量提升为全局环境变量，可供其他Shell程序使用   export 变量名

```
[root@hadoop101 datas]$ vim helloworld.sh 
# 在helloworld.sh文件中增加echo $B
#!/bin/bash
echo "helloworld"
echo $B
[root@hadoop101 datas]$ ./helloworld.sh 
Helloworld
# 发现并没有打印输出变量B的值。
[root@hadoop101 datas]$ export B
[root@hadoop101 datas]$ ./helloworld.sh 
helloworld
2
```

#### 2.3 特殊变量：$n

基本语法
$n （功能描述：n为数字，$0代表该脚本名称，$1-9 代表第一到第九个参数，十以上的参数，十以上的参数需要用大括号包含，如 9代表第一到第九个参数，十以上的参数，十以上的参数需要用大括号包含，如9代表第一到第九个参数，十以上的参数，十以上的参数需要用大括号包含，如{10}）

```
root@hadoop101 datas]$ touch parameter.sh 
[root@hadoop101 datas]$ vim parameter.sh
#!/bin/bash
echo "$0  $1   $2"
[root@hadoop101 datas]$ chmod 777 parameter.sh
[root@hadoop101 datas]$ ./parameter.sh cls  xz
./parameter.sh  cls   xz
```

#### 2.4 特殊变量：$#

基本语法
$# （功能描述：获取所有输入参数个数，常用于循环）。

```
[root@hadoop101 datas]$ vim parameter.sh
#!/bin/bash
echo "$0  $1   $2"
echo $#
[root@hadoop101 datas]$ chmod 777 parameter.sh
[root@hadoop101 datas]$ ./parameter.sh cls  xz
parameter.sh cls xz 
2
```

#### 2.5 特殊变量：∗ 、 *、∗、@

基本语法
∗ （功能描述：这个变量代表命令行中所有的参数， * （功能描述：这个变量代表命令行中所有的参数，∗（功能描述：这个变量代表命令行中所有的参数，*把所有的参数看成一个整体）
@ （功能描述：这个变量也代表命令行中所有的参数，不过 @ （功能描述：这个变量也代表命令行中所有的参数，不过@（功能描述：这个变量也代表命令行中所有的参数，不过@把每个参数区分对待）

```
[root@hadoop101 datas]$ vim parameter.sh
#!/bin/bash
echo "$0  $1   $2"
echo $#
echo $*
echo $@
[root@hadoop101 datas]$ bash parameter.sh 1 2 3
parameter.sh  1   2
3
1 2 3
1 2 3
```

#### 2.6 特殊变量：$？ 只有为0才为true

基本语法
$？ （功能描述：最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；如果这个变量的值为非0（具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了。）

```
[root@hadoop101 datas]$ ./helloworld.sh 
hello world
[root@hadoop101 datas]$ echo $?
0
```

### 3.运算符

基本语法
（1）“( ( 运算式 ) ) ”或“ ((运算式))”或“((运算式))”或“[运算式]”
（2）expr + , - , *, /, % 加，减，乘，除，取余
注意：expr运算符间要有空格

```
[root@hadoop101 datas]$ expr 3 - 2 
1
[root@hadoop101 datas]$ expr `expr 2 + 3` \* 4
20
[root@hadoop101 datas]# S=$[(2+3)*4] #20
[root@hadoop101 datas]# S=$(( (2+3)*5 )) #25
[root@hadoop101 datas]# echo $S
```

###  4.条件判断

#### 4.1 基本语法

[ condition ]（注意condition前后要有空格）
注意：条件非空即为true，[ root ]返回true，[] 返回false。

#### 4.2 常用判断条件

（1）两个整数之间比较
= 字符串比较
-lt 小于（less than） -le 小于等于（less equal）
-eq 等于（equal） -gt 大于（greater than）
-ge 大于等于（greater equal） -ne 不等于（Not equal）
（2）按照文件权限进行判断
-r 有读的权限（read） -w 有写的权限（write）
-x 有执行的权限（execute）
（3）按照文件类型进行判断
-f 文件存在并且是一个常规的文件（file）
-e 文件存在（existence） -d 文件存在并是一个目录（directory）

### 5.流程控制（重点）

#### 5.1 if 判断

基本语法
if [ 条件判断式 ];then
程序
fi
或者
if [ 条件判断式 ]
then
程序
fi
注意事项：
（1）[ 条件判断式 ]，中括号和条件判断式之间必须有空格
（2）if后要有空格

```
if [ $1 -eq "1" ]; then
echo "banzhang zhans s"
elif [ $1 -eq "2" ]
then
echo "skaskxax"
fi
```

#### 5.2 case 语句

基本语法
case $变量名 in
“值1”）
如果变量的值等于值1，则执行程序1
;;
“值2”）
如果变量的值等于值2，则执行程序2
;;
…省略其他分支…
*）
如果变量的值都不是以上的值，则执行此程序
;;
esac
注意事项：
1)case行尾必须为单词“in”，每一个模式匹配必须以右括号“）”结束。
2)双分号“;;”表示命令序列结束，相当于java中的break。
3)最后的“*）”表示默认模式，相当于java中的default。

```
case $1 in "1")
echo "11111";;
"2")
echo "222";;
*)
echo "not";;
esac
```

#### 5.3 for 循环

基本语法1
for (( 初始值;循环控制条件;变量变化 ))
do
程序
done

基本语法2

for 变量 in 值1 值2 值3…
do
程序
done

```
s=0
for((i=0;i<=100;i++))
do
   s=$[$s+$i]
done
echo $s
```

当它们被双引号“”包含时，“$*”会将所有的参数作为一个整体，以“$1 2 … 2 …2…n”的形式输出所有参数；“$@”会将各个参数分开，以“$1” “2 ” … ” 2”…”2”…”n”的形式输出所有参数。

```shell
[root@hadoop101 datas]$ vim for.sh
#!/bin/bash 
for i in "$*" 
#$*中的所有参数看成是一个整体，所以这个for循环只会循环一次 
do 
    echo "ban zhang love $i"
done 

for j in "$@" 
#$@中的每个参数都看成是独立的，所以“$@”中有几个参数，就会循环几次 
do 
	echo "ban zhang love $j" 
done
[root@hadoop101 datas]$ chmod 777 for.sh
[root@hadoop101 datas]$ bash for.sh cls xz bd
ban zhang love cls xz bd
ban zhang love cls
ban zhang love xz
ban zhang love bd
```

#### 5.4 while 循环

基本语法
while [ 条件判断式 ]
do
程序
done

```shell
[root@hadoop101 datas]$ touch while.sh
[root@hadoop101 datas]$ vim while.sh
#!/bin/bash
s=0
i=1
while [ $i -le 100 ]
do
        s=$[$s+$i]
        i=$[$i+1]
done
echo $s
[root@hadoop101 datas]$ chmod 777 while.sh 
[root@hadoop101 datas]$ ./while.sh 
5050
```

#### 5.5 read读取控制台输入

基本语法
read(选项)(参数)
选项：
-p：指定读取值时的提示符；
-t：指定读取值时等待的时间（秒）。
参数
变量：指定读取值的变量名

```shell
[root@hadoop101 datas]$ touch read.sh
[root@hadoop101 datas]$ vim read.sh
#!/bin/bash  在七秒内读取内容
read -t 7 -p "Enter your name in 7 seconds " NAME
echo $NAME
[root@hadoop101 datas]$ ./read.sh 
Enter your name in 7 seconds xiaoze
xiaoze
```

### 6.函数

#### 6.1系统函数

**1．basename基本语法**
basename [string / pathname] [suffix] （功能描述：basename命令会删掉所有的前缀包括最后一个（‘/’）字符，然后将字符串显示出来。
选项：suffix为后缀，如果suffix被指定了，basename会将pathname或string中的suffix去掉。

```shell
[root@hadoop101 datas]$ basename /home/root/banzhang.txt 
banzhang.txt
[root@hadoop101 datas]$ basename /home/root/banzhang.txt .txt
banzhang
```

**2.dirname基本语法**

dirname 文件绝对路径 （功能描述：从给定的包含绝对路径的文件名中去除文件名（非目录的部分），然后返回剩下的路径（目录的部分））

```shell
[root@hadoop101 ~]$ dirname /home/root/banzhang.txt 
/home/root
```

**3.自定义函数**

基本语法

```sh
[ function ] funname[()]
{
	Action;
	[return int;]
}
funname
```

+ 必须在调用函数地方之前，先声明函数，shell脚本是逐行运行。不会像其它语言一样先编译。
+ 函数返回值，只能通过$?系统变量获得，可以显示加：return返回，如果不加，将以最后一条命令运行结果，作为返回值。return后跟数值n(0-255)

```sh
#!/bin/bash
function sum()
{
    s=0
    s=$[ $1 + $2 ]
    echo "$s"
}
read -p "Please input the number1: " n1;
read -p "Please input the number2: " n2;
sum $n1 $n2;
[root@hadoop101 datas]$ chmod 777 fun.sh
[root@hadoop101 datas]$ ./fun.sh 
Please input the number1: 2
Please input the number2: 5
7
```

### 7.Shell工具（重点)

#### 7.1 cut

**基本用法**
cut [选项参数] filename
说明：默认分隔符是制表符
-f 列号，提取第几列
-d 分隔符，按照指定分隔符分割列

```sh
root@hadoop101 datas]$ vim cut.txt
dong shen
guan zhen
wo  wo
lai  lai
le  le
[root@hadoop101 datas]$ cut -d " " -f 1 cut.txt 
dong
guan
wo
lai
le
[root@hadoop101 datas]$ cut -d " " -f 2,3 cut.txt 
shen
zhen
wo
lai
le
```

在cut.txt文件中切割出guan

```sh
[root@hadoop101 datas]$ cat cut.txt | grep "guan" | cut -d " " -f 1
guan
```

选取系统PATH变量值，第2个“：”开始后的**所有路径**：

```sh
root@hadoop102 datas]$ echo $PATH | cut -d : -f 2-
/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin:/home/root/bin
```

切割ifconfig 后打印的IP地址

#### 7.2 sed

sed是一种流编辑器，它一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”，接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文件末尾。文件内容并没有改变，除非你使用重定向存储输出。

**基本用法**
sed [选项参数] ‘command’ filename
**选项参数说明**
-e 直接在指令列模式上进行sed的动作编辑。
**命令功能描述**
a 新增，a的后面可以接字串，在下一行出现
d 删除
s 查找并替换
4.案例实操

```
root@hadoop102 datas]$ vim sed.txt
dong shen
guan zhen
wo  wo
lai  lai

le  le
[root@hadoop102 datas]$ sed '2a mei nv' sed.txt 
dong shen
guan zhen
mei nv
wo  wo
lai  lai

le  le
[root@hadoop102 datas]$ cat sed.txt 
dong shen
guan zhen
wo  wo
lai  lai

le  le
# 注意：文件并没有改变
[root@hadoop102 datas]$ sed '/wo/d' sed.txt
# 将sed.txt文件中wo替换为ni
[root@hadoop102 datas]$ sed 's/wo/ni/g' sed.txt
# 将sed.txt文件中的第二行删除并将wo替换为ni
[root@hadoop102 datas]$ sed -e '2d' -e 's/wo/ni/g' sed.txt 
```



#### 7.3 awk

一个强大的文本分析工具，把文件逐行的读入，以空格为默认分隔符将每行切片，切开的部分再进行分析处理。
**基本用法**
awk [选项参数] ‘pattern1{action1} pattern2{action2}…’ filename
pattern：表示AWK在数据中查找的内容，就是匹配模式
action：在找到匹配内容时所执行的一系列命令
**选项参数说明**
-F 指定输入文件折分隔符

```sh
# 只显示/etc/passwd的第一列和第七列，以逗号分割，且在所有行前面添加列名user，shell在最后一行添加"dahaige，/bin/zuishuai"。
awk -F : 'BEGIN{print "user, shell"} {print $1","$7} END{print "dahaige,/bin/zuishuai"}' passwd
user, shell
root,/bin/bash
bin,/sbin/nologin
。。。
root,/bin/bash
dahaige,/bin/zuishuai
```

-v 赋值一个用户定义变量

```sh
[root@hadoop102 datas]$ awk -v i=1 -F: '{print $3+i}' passwd
```

**awk的内置变量**
FILENAME 文件名
NR 已读的记录数
NF 浏览记录的域的个数（切割后，列的个数）

```sh
# 统计passwd文件名，每行的行号，每行的列数
[root@hadoop102 datas]$ awk -F: '{print "filename:"  FILENAME ", linenumber:" NR  ",columns:" NF}' passwd 
# 切割IP
[root@hadoop102 datas]$ ifconfig ens160 | grep "inet6 " | awk -F " " '{print $2}'
192.168.40.130
# 查询sed.txt中空行所在的行号
[root@hadoop102 datas]$ awk '/^$/{print NR}' sed.txt 
```

#### 7.4 sort

sort命令是在Linux里非常有用，它将文件进行排序，并将排序结果标准输出。
**基本语法**
sort(选项)(参数)
**选项 说明**
-n 依照数值的大小排序
-r 以相反的顺序来排序
-t 设置排序时所用的分隔字符
-k 指定需要排序的列
参数：指定待排序的文件列表

```sh
[root@hadoop102 datas]$ vim sort.sh 
bb:40:5.4
bd:20:4.2
xz:50:2.3
cls:10:3.5
ss:30:1.6
# 按照“：”分割后的第三列倒序排序。
[root@hadoop102 datas]$ sort -t : -nrk 3  sort.sh 
bb:40:5.4
bd:20:4.2
cls:10:3.5
xz:50:2.3
ss:30:1.6
```

## 8.正则表达式

#### **基础正则表达式元字符**

| 元字符     | 作用                                                         |
| ---------- | ------------------------------------------------------------ |
| \          | 转义字符，用于取消特殊符号的含义，如：\！、\n等              |
| ^          | 匹配字符串的开始位置，如：^world匹配以world开头的行          |
| $          | 匹配字符串的结束位置，如：world$匹配以world结尾的行          |
| .          | 匹配除\n（换行）之外的任意一个字符                           |
| *          | 匹配前面的子表达式0次或者多次                                |
| [list]     | 匹配list列表中的一个字符，如：[0-9]匹配任一位数字            |
| [^list]    | 匹配不在list列表中的一个字符，如：[^0-9]匹配任意一位非数字字符 |
| \ {n \ }   | 匹配前面的子表达式n次，如：[0-9] \ {2 \ }匹配两位数字        |
| \ {n, \ }  | 匹配前面的子表达式不少于n次，如：[0-9]{2,\ }表示两位及两位以上数字 |
| \ {n,m \ } | 匹配前面的子表达式n到m次，如：[a-z]\ {2,3 \ }匹配两到三位的小写字母 |

#### 扩展正则表达式元字符

| 元字符 | 作用              |
| ------ | ----------------- |
| +      | 匹配1~n个前导字符 |
| ？     | 匹配0~1个前导字符 |
| a I b  | 匹配a或b          |
| ()     | 组字符            |

#### 正则表达式元字符综合案例

```
^$                              #空行
^[A-Z]..$                       #开头一个大写，最后两个任意字符
^[A-Z][a-z]*3[0-5]		        #开头一个大写，0到多个小写，3最后是0-5的一个数字
 [a-z]*\.                       #0到多个小写字母最后一个点
^ *[A-Z][a-z][a-z]$             #0到多个空格开头一个大写一个小写再以一个小写结尾 
^[A-Za-z]*[^,][A-Za-z]*$		#0到多个字母开头，但不是逗号，0到多个字母结尾
\<fourth\>				        #找个单词
\<f.*th\>				        #找个单词
5{2}2{3}\.				        #两个五  三个二最后一个点
^[ \t]*$				        #0到多个空格或tab的行
^# 						        #以#开头的注释行
^[ \t]*#				        #0到多个空格或者tab开头的注释行
:1,$ s/\([Oo]ccur\)ence/\1rence/  
```

