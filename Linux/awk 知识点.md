#### AWK 介绍
`AWK` 是一种优良的文本处理工具，`Linux` 及 `Unix` 环境中现有的功能最强大的数据处理引擎之一。

`AWK` 是一种处理文本文件的语言。它将文件作为记录序列处理。在一般情况下，文件内容的每行都是一个记录。每行内容都会被分割成一系列的域，因此，我们可以认为一行的第一个词为第一个域，第二个词为第二个，以此类推。`AWK` 程序是由一些处理特定模式的语句块构成的。AWK一次可以读取一个输入行。对每个输入行，AWK解释器会判断它是否符合程序中出现的各个模式，并执行符合的模式所对应的动作。

#### AWK 实战
实战内容来自酷壳：https://coolshell.cn/articles/9070.html
##### 1.  `netstat` 命令中提取了如下信息 到 `netstat.txt` 文件
```
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 VM_0_15_centos:46174    169.254.0.55:lsi-bobcat ESTABLISHED
tcp        0      0 VM_0_15_centos:ssh      123.123.135.226:13481   ESTABLISHED
tcp        0      0 VM_0_15_centos:ssh      185.127.126.233:51604   ESTABLISHED
udp        0      0 VM_0_15_centos:48963    183.60.82.98:domain     ESTABLISHED
udp        0      0 VM_0_15_centos:44597    183.60.83.19:domain     ESTABLISHED
Active UNIX domain sockets (w/o servers)
Proto RefCnt Flags       Type       State         I-Node   Path
unix  3      [ ]         DGRAM                    7440     /run/systemd/notify
unix  2      [ ]         DGRAM                    7442     /run/systemd/cgroups-agent
unix  6      [ ]         DGRAM                    7463     /run/systemd/journal/socket
unix  13     [ ]         DGRAM                    7465     /dev/log
unix  3      [ ]         STREAM     CONNECTED     77097119 /usr/local/yd.socket.client
unix  2      [ ]         DGRAM                    10975    /run/systemd/shutdownd
unix  3      [ ]         STREAM     CONNECTED     12851
unix  2      [ ]         DGRAM                    12842
unix  3      [ ]         STREAM     CONNECTED     78243738 /run/systemd/journal/stdout
unix  2      [ ]         DGRAM                    13298
```

##### 2. 执行命令输入第 1 列和第 4 列
- 其中单引号中的被大括号括着的就是awk的语句，注意，其只能被单引号包含。
- 其中的$1、$n表示第几例。注：$0表示整个行。

执行命令：`awk '{print $1,$4}' netstat.txt`

执行结果：
```
Active (w/o
Proto Local
tcp VM_0_15_centos:46174
tcp VM_0_15_centos:ssh
tcp VM_0_15_centos:ssh
udp VM_0_15_centos:48963
udp VM_0_15_centos:44597
Active sockets
Proto Type
unix ]
unix ]
unix ]
unix ]
unix ]
unix ]
unix ]
unix ]
unix ]
unix ]
```

##### 3. awk的格式化输出，和C语言的printf没什么两样

执行命令： `awk '{printf "%-8s,,,%-8s\n",$1,$5}' netstat.txt`

执行结果：
```
Active  ,,,servers)
Proto   ,,,Address
tcp     ,,,169.254.0.55:lsi-bobcat
tcp     ,,,123.123.135.226:13481
tcp     ,,,185.127.126.233:51604
udp     ,,,183.60.82.98:domain
udp     ,,,183.60.83.19:domain
Active  ,,,(w/o
Proto   ,,,State
unix    ,,,DGRAM
unix    ,,,DGRAM
unix    ,,,DGRAM
unix    ,,,DGRAM
unix    ,,,STREAM
unix    ,,,DGRAM
unix    ,,,STREAM
unix    ,,,DGRAM
unix    ,,,STREAM
unix    ,,,DGRAM
```

##### 4. 使用运算符过滤记录
执行命令： `awk '$2==3 && $5=="DGRAM"' netstat.txt`

执行结果：
```
unix  3      [ ]         DGRAM                    7440     /run/systemd/notify
```
如果我们需要表头的话，我们可以引入内建变量`NR`：

执行命令： `awk '$2==3 && $5=="DGRAM" || NR==1' netstat.txt`

执行结果：
```
Active Internet connections (w/o servers)
unix  3      [ ]         DGRAM                    7440     /run/systemd/notify
```

再加上格式化输出：

执行命令： `awk '$2==3 && $5=="DGRAM" || NR==1 {printf "%-20s %-20s\n", $1,$5}' netstat.txt`

执行结果：
```
Active               servers)
unix                 DGRAM
```

##### 5. AWK 内建变量
|内建变量  | 释义
| --- | --- |
| $0 | 当前记录（这个变量中存放着整个行的内容） |
| $1~$n | 当前记录的第n个字段，字段间由FS分隔 |
| FS | 输入字段分隔符 默认是空格或Tab |
| NF | 当前记录中的字段个数，就是有多少列 |
| NR | 已经读出的记录数，就是行号，从1开始，如果有多个文件话，这个值也是不断累加中。 |
| FNR | 当前记录数，与NR不同的是，这个值会是各个文件自己的行号 |
| RS | 输入的记录分隔符， 默认为换行符 |
| OFS | 输出字段分隔符， 默认也是空格 |
| ORS | 输出的记录分隔符，默认为换行符 |
| FILENAME | 当前输入文件的名字 |

输出行号

执行命令（单个文件）：`awk '$2==3 && $5=="DGRAM" || NR==1 {printf "%-20s %-20s %-20s %-20s\n", NR, FNR, $1 ,$5}' netstat.txt`

执行结果：
```
1                    1                    Active               servers)
10                   10                   unix                 DGRAM
```

执行命令（多个文件）：`awk '$2==3 && $5=="DGRAM" || NR==1 {printf "%-20s %-20s %-20s %-20s\n", NR, FNR, $1 ,$5}' qqq.txt netstat.txt`

执行结果：
```
1                    1                    Active               servers)
38                   10                   unix                 DGRAM
```

##### 6. 指定分隔符

执行命令：`awk  'BEGIN{FS=":"} {print $1,$3,$6}' /etc/passwd`

等价于上面命令：`awk -F: '{print $1,$3,$6}' /etc/passwd`

执行结果：
```
root 0 /root
bin 1 /bin
daemon 2 /sbin
adm 3 /var/adm
lp 4 /var/spool/lpd
sync 5 /sbin
shutdown 6 /sbin
halt 7 /sbin
mail 8 /var/spool/mail
operator 11 /root
games 12 /usr/games
ftp 14 /var/ftp
```
以 `\t 制表符tab` 作为分隔符输出的例子，执行命令：`awk -F: '{print $1,$3,$5}' OFS="\t" /etc/passwd`

执行结果：
```
root	0	root
bin	1	bin
daemon	2	daemon
adm	3	adm
lp	4	lp
sync	5	sync
shutdown	6	shutdown
halt	7	halt
mail	8	mail
operator	11	operator
games	12	games
ftp	14	FTP User
```
