---
title: linux命令大全
date: 2019-12-28 18:02:27
tags: 
    - linux
    - shell
categories: 技术
---

## linux终端快捷键

`ctrl+l`: 清屏

`ctrl+c`: 中止

`ctrl+d`： 退出

<!--more-->

`ctrl+a`: 光标移到命令行的头部

`ctrl+e`: 光标移到命令行的尾部

`ctrl+u`: 删除光标前所有字符

`ctrl+k`: 删除光标后所有字符

`ctrl+y`: 撤销

`ctrl+r`: 搜索历史命令

`ctrl+z`：暂停前台进程

`esc+.`: 上一条命令的最后一个参数

`up/down`：查看历史命令

`ctrl+R`：搜索历史命令

## shell命令

### 基础命令

#### histroy/!

`history`: 历史命令

`!20`：执行第20条历史命令

`!str`：执行历史最近中以str开头的命令

`!$`：上一个命令的最后一个参数

`!!`：执行上一个命令


#### man

`man command`: command手册

`man -f command`: 查看command的所在所有章节

`man 1 command`: 查看第1章节中的command手册

#### alias

`alias ll=ls -l`: 别名

`unalias ll`: 取消别名

* 查看命令的别名

    `type -a ls`: 查看所有`ls`命令的类型

    ```shell
    # 上述命令的输出
    ls is aliased to `ls --color=auto'
    ls is /bin/ls
    ```

    `ls`： 系统默认使用别名
    `\ls`: 使用原ls命令

#### echo

`echo "123"`：返回或者输出123，会输出换行

`echo -n "123"`：不换行输出

#### which/whereis

`which command`: 查看命令的目录（根据`PATH`目录）

`whereis command`: 整个文件索引数据库查找命令

### 用户操作

`id username`: 查看用户

`whoami`: 显示当前用户

`useradd cetrol`: 添加用户

#### groupadd/groupdel/useradd/userdel

`groupadd groupname` 添加

`userdel [-r] username`: 删除用户（去掉home目录）

#### passwd/gpasswd

`passwd username`: 为用户设置密码

`gpasswd -a username groupname`: 将用户添加到组

`gpasswd -d username groupname`: 将用户移除组

### 目录与文件操作

`pwd`: 当前工作目录

#### ls

`ls`：当前目录内容

`ls -a`: list all 所有，包含隐藏文件

`ls dir`: dir目录下的内容

`ll -h`: 人性化显示

`ll -t`: 按照时间排序，最新在前

`ll -rt`: reverse，逆序

`ll dir`: 查看目录下所有文件的详情

`ll -d dir`: 只查看目录的详情

#### cd

`cd`: 到`home`目录

`cd ~`: 到`home`目录

`cd dir`: 到`dir`目录

`cd -`: 回到上一目录

#### mkdir

`mkdir dir1, dir2`: 创建目录

`mkdir -v dir`: verbose，显示详细信息

`mkdir -p dir`: 如果父目录不存在，会自动创建父目录

#### touch

`touch file1 file2`: 创建文件

`touch file{1..20}`: 创建多个文件

#### cp

`cp file1 file2`: 拷贝文件

`cp -r dir1 dir2`: 递归拷贝目录

#### mv

`mv file1 file2`: 剪切文件

#### rm

`rm -rf dir`: 递归删除目录

#### chmod/chown/chgrp

|    u     |    g     |     o      |      |       |
| :------: | :------: | :--------: | :--: | :---: |
|   rwx    |   rw-    |    r--     | user | group |
| 属主权限 | 属组权限 | 其他人权限 | 属主 | 属组  |

`chown [-r] user file/dir`: 改变file的所属组（递归修改目录）

`chgrp [-r] group file/dir`: 改变file的所属组（递归修改目录）

`chown [-r] user.group file/dir`: 同时修改主和组

`chmod u/g/o+w/r/x file`: 相应的u/g/o添加w/r/x权限

`chmod [a]+w/r/x file`: 所有组添加w/r/x权限

`chmod ug=rw file`: u和g添加rw权限

`chmod o=-`: o去掉所有权限

`chmod 777 file`: 所有组添加rwx权限

#### tar

`tar -czf test.tar.gz dir`: 调用gzip将`dir`目录打包并压缩为`test.tar.gz`

`tar -cjf test.tar.bz2 dir`: 调用bzip2将`dir`目录打包并压缩为`test.tar.bz2`

`tar -cJf test.tar.xz dir`: 调用xz将`dir`目录打包并压缩为`test.tar.xz`

`tar -tf test.tar.xz`: 显示压缩包中的内容

`tar -xzvf test.tar.gz`:  指定使用gzip解压

`tar -xvf test.tar.gz`: 自行判断解压工具解压

`tar -xvf test.tar.gz -C dir`: 解压后重定向到`dir`目录下

`tar -czf - /etc | tar -xzf - -C dir `: 将/etc打包后保存在内存中，然后通过管道解压后`dir`目录下；相当于复制文件到另一个目录（许多小文件复制时速度快）

#### du

`du -h dir`: 查看目录`dir`下所有文件夹的大小

`du -sh dir`: 查看目录`dir`总大小

### 文件增删改查

#### cat

`cat file`: 查看文本文件

`cat -n file`: 显示行号

#### head/tail

`head -5 file`: 查看头5行

`tail -5 file`: 查看尾5行

`tail -f file`: 动态查看

#### vi/vim

`vi file`: 编辑文件

#### sort

`sort file`: 按照字典序递增排序

`sort -r file`: 递减排序

`sort -n file`: 按照数字大小排序

`sort -t":" -k3 -n /etc/passwd`: 以`:`分割后按照第3列排序

#### uniq

`uniq`: 去重（只能把接近的去掉，所以一般先`sort`）

`uniq -c`: 去重，并显示个数

#### wc

`wc -l`: 统计行数

#### tr

> tr(translate) 可以用来替换、删除文本字符内容

`tr -d a file`: 将文本中所有字符`a`删掉

`tr -d a-z file`: 将文本中所有小写字母删掉

`tr -d ab file`: 将文本中字符`a`和`b`删掉

`tr -s a file`: 将文本中连续出现多个的字符`a`压缩成单个字符`a`

`tr -s ab file`: 将文本中连续出现多个字符`a`，多个字符`b`分别压缩成单个字符`a`和`b`

`tr -s a b file`: 将文本中连续出现多个字符`a`压缩并替换成单个字符`b`

`tr a b file`: 将文本中的每个字符`a`替换成`b`

`tr -s ab c file`: 将文本中连续出现多个字符`a`, 多个字符`b`压缩并替换成单个字符`c`

`tr ab c file`: 将文本中每个字符`a`,`b`替换成`c`

`tr ab cd file`: 将文本中每个字符`a`,`b`分别替换成`c`和`d`

`tr -s a-z A-Z file`: 将文本中连续出现多个字符`a`, 多个字符`b`分别压缩并替换成单个字符`c`和单个字符`d`

`tr a-z A-Z file`: 将文本中每个字符`a`, `b`分别替换成`c`和`d`

`tr -c -d 'a\n' file`: 将文本中除了`a`和`\n`外的字符都删掉

#### awk

> awk是一种编程语言，用与逐行处理文本和数据

`awk -F":" '{print $1}' /etc/passwd`: 以`:`分割输出第1列

`awk 'BEGIN{} {} END{}'`：文件处理前 行处理 文件处理后

`awk 'BEGIN{FS=":",OFS="---"} {print $1,$2}'`：输入字段以`:`分割，输出字段之间`---`输出，`FS`为输入分割符，`OFS`为输出字段分割符，`OFS`默认值为空格

`awk 'BEGIN{RS=":",ORS="---"} {print $1,$2}'`：输入行以`:`分割，输出行之间`---`输出，`RS`为记录分割符，`ORS`为输出记录分割符，`RS`和`ORS`默认值为换行符

`awk pattern filename`：正则匹配输出

`awk '/root/{action}' filename`：匹配到`root`的行再执行`action`

`awk '{print NR,FNR}'`：`NR`表示总的记录行号，FNR表示当前文件的记录行号

`awk '{print NF}'`：`NF`表示字段的个数

`awk '{printf "%-15s %-d %-f", $1,$2,$3}'`：`printf`格式化输出，`%s`表示字符串，`%d`表示整形数据，`%f`表示浮点数，`-`表示左对齐，`15`表示长度为15

`awk '$0~/root/' /etc/passwd`：正则匹配行，等效于`awk '/root/ /etc/passwd'`

`awk '$0!~/root/' /etc/passwd`：正则不匹配，等效于`awk '!/root/ /etc/passwd'`

`awk '$1=="root"' /etc/passwd`：关系运算符

`awk '$1>100' /etc/passwd`：关系运算符

`awk '{if($3>100){print $3} else if(){} else{print $1}}'`：条件运算符

`awk '{$1*10}'`：算术运算符

`awk '{if($1>10 && $2<10){print $0}}'`：条件运算符

`awk {length($1)==10 {print $1}}`：`length`函数表示字符串长度

`awk '{i=1; while(i<=7) {print $i; i++}}'`：while循环

`awk '{for(i=1;i<=NF;i++) {print $i}}'`：for循环

`awk '{color[++i]}=$1' END{for(i=1;i<i;i++) {print i,color[i]}}`：数组

`awk '{color[++i]}=$1' END{(for i in color) {print i,color[i]}}`：数组，`for`循环使用`in`遍历获取的是索引


#### sed

> sed是一种非交互式编辑器，一次处理一行内容

`sed action file`：从文件`file`中每次读入一行，执行`action`动作，不会修改文件内容，只做输出

`sed -i action file`：会修改文件内容

`sed "d" file`：删除输出（相当于什么都不输出）

`sed "4d" file`：删除第四行

`sed "3{d;}"`：`{}`里面的命令按照`;`分割，可以执行多个目录

`sed "4,7d" file`：删除第4到7行

`sed "4,$d" file`：删除第4到最后一行

`sed -r "0~2d"`：从0开始，每2行删除，（相当于删除偶数行）

`sed -r /root/,5d file`：删除从root到第5行

`sed -r /root/,+5d file`：删除root，再删其后5行

`sed -r /root/!d file`：删除非root行

`sed "p" file`：打印输出（默认）

`sed "4p" file`：只打印第4行输出（默认）

`sed -r [s]/sth./sth./[gi]`：使用正则匹配

`sed -r "s/red/yellow/" file`：查找`red`并替换为`yellow`，只替换每一行第一个

`sed -r "s/red/yellow/g file"`：加上`g`，表示global，替换所有

`sed -r "s/red/yellow/g file"`：加上`i`，表示ignore-case，忽略大小写

`sed -r "s#red#yellow#g file"`：`/`其实可以替换成任意字符

`sed -r "3,5s/red/yellow/g file"`：只有3到5行替换

`sed -r "s/(.*)/#&/ file"`：所有行前面加入`#`，`(.*)`表示匹配行所有内容，`&`表示前面匹配的所有内容

`sed -r "2r/new_file" file`：读到第2行时读取文件内容，`r`表示读入

`sed -r "2w/new_file" file`：读到第2行时写入到新文件，`w`表示读入

`sed -r "2a/new_file" file`：读到第2行时追加新内容或到新文件，`a`表示追加（行后）

`sed -r "2i/new_file" file`：读到第2行时插入新内容或到新文件，`i`表示插入（行前）

`sed -r "/root/{n;d} file"`：找到root行，然后去下一行并删除 ，`n`表示下一行

### 时间相关命令

#### date

`date +%F`:  格式化输出时间

#### time

`time command`: 显示命令执行的时间

### 进程相关命令

#### ps/top

`ps aux`: 查看进程运行状态(快照模式)（process state）

`ps aux --sort [-]%cpu`: 以cpu占用排序查看进程状态

`ps auxf`: 加上f，显示进程的层级关系

`ps -ef`: 查看PID（进程ID）和PPID（进程父ID）

`top`: 动态模式

#### kill

`kill -1 PID`: 1为重启信号

`kill [-15] PID`:  默认为15即停止信号

`kill -9 PID`: 9为强制关闭信号

#### fg/bg/jobs

`jobs`：查看所有后台进程

`fg pid`：将后台pid任务切换至前台运行

`bg pid`：将前台pid任务切换至后台运行

### 硬件相关命令

#### lscpu

`lscpu`: 查看cpu信息

#### lsblk

`lsblk`: 显示硬盘分区

#### fdisk/gdisk/partprobe

`fdisk`: MBR分区工具

`fdisk /dev/sda`: 硬盘分区工具

`fdisk -l /dev/sda`: 查看分区信息

`gdisk`: GPT分区工具

`gdisk /dev/sda`: 硬盘分区工具

`gdisk -l /dev/sda`: 查看分区信息

`partprobe`：让kernel读取分区信息

#### mkfs

`mkfs`: 创建文件系统（格式化）

`mkfs.ext4 /dev/sda1`: 将硬盘sda的分区1格式化为ext4格式

#### mount/umount

`mount /dev/sda1 dir` 将设备挂载到目录下`dir`下

> mount命令只是临时挂载，长期挂载需在/etc/fstab文件中设置

`mount -a`: 从`/etc/fstab`中查找并挂载所有

`umount /dev/sda1`: 

#### df

`df`: 查看分区信息

`df -T`: 显示分区文件系统格式

`df -h`: 以`G,T`显示分区大小

### 管道相关命令

#### >/>>/</<<

| 描述符 | 含义     | 对应文件     |
| ------ | -------- | ------------ |
| 0      | 标准输入 | 键盘设备文件 |
| 1      | 标准输出 | 终端设备文件 |
| 2      | 标准错误 | 终端设备文件 |

`ls 1>out.txt 2>err.txt`: 标准输出到`out.txt`，错误输出到`err.txt`

`ls >out.txt`: 等效于`ls 1>out.txt`: 标准输出到`out.txt`

`ls >>out.txt`: 追加到文件

`ls &>out.txt`: 混合输出（包括标准输出和错误输出）

`ls 2>&1`: 错误输出（2）到标准输出（设备描述符1）中

`ls &>/dev/null`: 混合输出到空设备（垃圾桶）

`ls 2>/dev/null`: 错误输出到空设备

`cat >out.txt`: 将终端输入重定向到`out.txt`

`cat >out.txt <<EOF`: 输入直到遇到`EOF`结束输入

`grep 'root' < /etc/passwd`: 输入重定向，`grep 'root'`的输入重定向到`/etc/passwd`

#### |/mkfifo

`|`: 匿名管道

`command 1 | command 2`: 管道，命令1的输出重定向为命令2的输入

`mkfifo /tmp/fifo1`：命名管道，可以跨终端传输数据

#### tee

`ls -l | tee result.txt`：`ls -l`的结果不仅输出到`result.txt`中，而且会显示到屏幕上

`ls -l | tee -a result.txt`：追加

### 搜索相关命令

#### locate/find

`locate string`: 从数据库中查找包含`string`的文件

> 数据库为/var/lib/mlocate/mlocate.db，可以使用`updatedb`命令手动更新该数据库

`find dir`: 查找`dir` 目录下所有文件（显示所有文件）

`find dir [-name] "string"`: 查找`dir`目录下包含`string`的所有文件

`find dir -iname "string"`: 查找文件，忽略大小写

`find dir -size +5M: 查找大于`5M`的文件

`find dir -maxdepth 3 -a -name "string"`:  查找文件且深度为3（`-a`表示and，`-o`表示or）

`find dir -mtime +5`: 修改时间超过5天内的文件

`find dir -user username`: 用户为`username`的文件

`find dir -group groupname`: 查看组为`groupname`的文件

`find dir -nouser -o -nogroup`: 没用户没组的文件

`find dir ! -user username`:  `!`表示取反

`find dir -type d`: 查找为`d`（目录）的文件

`find dir -perm 666`: 查找权限大于等于666的文件

`find dir -regex 'regular expression'`: 以正则表达式查找 

`find {sth} -delete`: 查找后删除

`find {sth} -print/-ls`: 查找后打印（`ls`更详细）

`find {sth} -exec/-ok command \;` : 查找后执行`command` shell命令（`-ok`有交互，会询问是否如此操作）

### grep正则命令

#### grep/egrep

`grep pattern file1 [file2]`：从文件中过滤出匹配到模式的字符串

`grep -q pattern file1`：不打印结果，只返回输出码（0表示找到，1表示没找到，2表示文件不存在）

`grep -v`：取反，未匹配到的打印输出

`grep -i`：忽略大小写

`grep -o`：只输出匹配到的内容

`grep -n`：输出匹配到的行号

`grep -r[-R] dir`：递归遍历`dir`目录匹配

`grep -E(egrep)`：`egrep`可以直接使用扩展元字符`?`，`+`，`{}`，`|`，`()`，但是在`grep`中需要使用`\`转义才能使用扩展元字符

### 其他

#### screen

`screen -S session_name`: 新开一个名字叫session_name的screen作业

`screen -ls`: 查看所有screen作业

`screen -r`: 恢复上一次的作业

`screen -r session_name`: 恢复session_name的作业（attached）

`screen -d`: 离线当前作业（detached）

`ctrl+a d`：离线当前作业

`screen -d session_name`: 离线session_name作业（detached）

`exit`: 在attached的screen作业下输入该命令，终止该screen作业

`kill pid`: `pid`为screen的进程id，终止该screen作业

`screen -wipe`: 清理掉没用的（deaded）screen作业

#### seq

`seq 10`：产生1到10的序列

`seq 2 10`：产生从2到10的序列

`seq 1 2 10`：产生从1到10的序列，递增为2






