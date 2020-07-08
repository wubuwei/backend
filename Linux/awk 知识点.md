#### AWK 介绍
介绍内容来自网络搜索：
`AWK` 是一种优良的文本处理工具，`Linux` 及 `Unix` 环境中现有的功能最强大的数据处理引擎之一。

`AWK` 是一种处理文本文件的语言。它将文件作为记录序列处理。在一般情况下，文件内容的每行都是一个记录。每行内容都会被分割成一系列的域，因此，我们可以认为一行的第一个词为第一个域，第二个词为第二个，以此类推。`AWK` 程序是由一些处理特定模式的语句块构成的。`AWK` 一次可以读取一个输入行。对每个输入行，`AWK` 解释器会判断它是否符合程序中出现的各个模式，并执行符合的模式所对应的动作。

##### awk、nawk、mawk、gawk
- `awk`: 最初在 `1977` 年完成，`1985` 年发表了一个新版本的 `awk`，它的功能比旧版本增强了不少,`awk` 能够用很短的程序对文档里的资料做修改、比较、提取、打印等处理,如果使用 `C` 或 `Pascal` 等语言编写程序完成上述的任务会十分不方便而且很花费时间，所写的程序也会很大;

- `nawk`： 在 `20` 世纪 `80` 年代中期，对 `awk` 语言进行了更新，并不同程度地使用一种称为 `nawk(new awk)` 的增强版本对其进行了替换。许多系统中仍然存在着旧的 `awk` 解释器，但通常将其安装为 `oawk (old awk)` 命令，而 `nawk` 解释器则安装为主要的 `awk` 命令，也可以使用 `nawk` 命令。`Dr. Kernighan` 仍然在对 `nawk` 进行维护，与 `gawk` 一样，它也是开放源代码的，并且可以免费获得;

- `mawk`：`mawk` 是 `awk` 编程语言的解释器。`awk` 语言在多媒体数据文件以及文本的检索和处理，算法的原型设计和试验都有广泛的使用。`mawk` 带给 `awk` 新的概念，它实现了在`《The AWK Programming Language》（Aho, Kernighan and Weinberger, The AWK Programming Language, Addison-Wesley Publishing, 1988.被认为是 AWK 手册。）` 中定义的 `awk` 语言。`mawk` 遵循 `POSIX 1003.2 （草案 11.3）定义的 AWK 语言`，包含了一些没有在 `AWK` 手册中提到的特色，同时 `mawk` 提供一小部分扩展,另外据说 `mawk` 是实现最快的 `awk`；

- `gawk`： 是 `GNU Project` 的 `awk` 解释器的开放源代码实现。尽管早期的 `GAWK` 发行版是旧的 `AWK` 的替代程序，但不断地对其进行了更新，以包含 `NAWK` 的特性;

目前，大家都比较倾向于使用 `awk` 和 `gawk`。

#### AWK 实战
实战内容来自：
- 酷壳：https://coolshell.cn/articles/9070.html
- 阮一峰：http://www.ruanyifeng.com/blog/2018/11/awk.html

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
命令描述：以 `-F` 后面的 `:` 作为分隔符
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

##### 7. 字符串匹配

执行命令：`awk '$6 ~ /ESTA/ || NR==1 {print NR,$4,$5,$6}' OFS="\t" netstat.txt`
命令解释：`~` 表示模式开始。`/ /` 中是模式。这就是一个正则表达式的匹配。
执行结果：
```
1	(w/o	servers)
3	VM_0_15_centos:46174	169.254.0.55:lsi-bobcat	ESTABLISHED
4	VM_0_15_centos:ssh	123.123.135.226:13481	ESTABLISHED
5	VM_0_15_centos:ssh	185.127.126.233:51604	ESTABLISHED
6	VM_0_15_centos:48963	183.60.82.98:domain	ESTABLISHED
7	VM_0_15_centos:44597	183.60.83.19:domain	ESTABLISHED
```


执行命令：`awk '$6 !~ /ESTA/ || NR==1 {print NR,$4,$5,$6}' OFS="\t" netstat.txt`
命令解释：`!` 表示模式取反
或者执行命令：`awk '!/ESTA/ {print NR,$4,$5,$6}' OFS="\t" netstat.txt`
执行结果：
```
1	(w/o	servers)
2	Local	Address	Foreign
8	sockets	(w/o	servers)
9	Type	State	I-Node
10	]	DGRAM	7440
11	]	DGRAM	7442
12	]	DGRAM	7463
13	]	DGRAM	7465
14	]	STREAM	CONNECTED
15	]	DGRAM	10975
16	]	STREAM	CONNECTED
17	]	DGRAM	12842
18	]	STREAM	CONNECTED
19	]	DGRAM	13298
```

##### 8. 折分文件

命令介绍：按第 `6` 例分隔文件，第 `6` 列一样的将被打印在一个文件中，文件名是这列的值
执行命令：`awk '$6 !~ /ESTA/ || NR==1 {print NR,$4,$5,$6}' OFS="\t" netstat.txt`
命令解释：`NR!=1` 表示不处理表头
执行结果：`awk 'NR!=1{print > $6}' netstat.txt`

命令介绍：将 `4`、`5` 列输出到第 `6` 列值为文件名的文件中
执行命令：`awk 'NR!=1{print $4,$5 > $6}' netstat.txt`


##### 9. 统计文件

命令介绍：计算所有的 `txt` 文件，`log` 文件和 `c` 文件的文件大小总和
执行命令：`ls -l *.txt *.c *.log | awk '{sum+=$5} END {print sum}'`
命令解释：`ls -l` 展示的第 `5` 列表示文件大小

命令介绍：统计用户的进程的占了多少内存（加起来的是 `ps aux` 展示的 `RSS` 列）
执行命令：`ps aux | awk 'NR!=1{a[$1]+=$6;} END { for(i in a) print i ", " a[i]"KB";}'`
执行结果：
```
sshd, 2208KB
www, 156968KB
polkitd, 1692KB
dbus, 1448KB
named, 100412KB
nscd, 1796KB
libstor+, 4KB
mysql, 173364KB
ntp, 784KB
root, 168048KB
```

#### 10. 编写 awk 脚本

`END` 和 `BEGIN` 关键词介绍
- `BEGIN`{ 这里面放的是执行前的语句 }
- `END` {这里面放的是处理完所有的行后要执行的语句 }
- {这里面放的是处理每一行时要执行的语句}

新建学生成绩表
```
$ cat score.txt
Marry   2143 78 84 77
Jack    2321 66 78 45
Tom     2122 48 77 71
Mike    2537 87 97 95
Bob     2415 40 57 62
```
创建 awk 脚本
```
$ cat cal.awk
#!/bin/awk -f
#运行前
BEGIN {
    math = 0
    english = 0
    computer = 0

    printf "NAME    NO.   MATH  ENGLISH  COMPUTER   TOTAL\n"
    printf "---------------------------------------------\n"
}
#运行中
{
    math+=$3
    english+=$4
    computer+=$5
    printf "%-6s %-6s %4d %8d %8d %8d\n", $1, $2, $3,$4,$5, $3+$4+$5
}
#运行后
END {
    printf "---------------------------------------------\n"
    printf "  TOTAL:%10d %8d %8d \n", math, english, computer
    printf "AVERAGE:%10.2f %8.2f %8.2f\n", math/NR, english/NR, computer/NR
}
```
执行命令：`awk -f cal.awk score.txt`
命令描述：`-f` 表示从脚本文件中读取 `awk` 命令。
执行结果：
```
NAME    NO.   MATH  ENGLISH  COMPUTER   TOTAL
---------------------------------------------
Marry  2143     78       84       77      239
Jack   2321     66       78       45      189
Tom    2122     48       77       71      196
Mike   2537     87       97       95      279
Bob    2415     40       57       62      159
---------------------------------------------
  TOTAL:       319      393      350
AVERAGE:     63.80    78.60    70.00
```

##### 11. 内置函数

执行命令：`awk '{print toupper($7)}' netstat.txt`
命令描述：将第 `7` 列置为大写展示
执行结果：
```
ADDRESS






PATH
/RUN/SYSTEMD/NOTIFY
/RUN/SYSTEMD/CGROUPS-AGENT
/RUN/SYSTEMD/JOURNAL/SOCKET
/DEV/LOG
77097119
/RUN/SYSTEMD/SHUTDOWND
12851

78243738
```
常用内置函数：
- toupper: 字符转为大写
- tolower: 字符转为小写
- length: 返回字符串长度
- substr: 返回子字符串
- sin: 正弦
- cos: 余弦
- sqrt: 平方根
- rand: 随机数

##### 12. if 语句

执行命令：`awk -F: '{if ($3>6) print $0; else print "-------"}' /etc/passwd`
命令描述：paswd 文件内以 `:` 分隔，若第 3 列大于 6 展示整行，否则打印`-------`
执行结果：
```
-------
-------
-------
-------
-------
-------
-------
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:999:998:User for polkitd:/:/sbin/nologin
libstoragemgmt:x:998:997:daemon account for libstoragemgmt:/var/run/lsm:/sbin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
ntp:x:38:38::/etc/ntp:/sbin/nologin
abrt:x:173:173::/etc/abrt:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
chrony:x:997:995::/var/lib/chrony:/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
syslog:x:996:994::/home/syslog:/bin/false
dockerroot:x:995:992:Docker User:/var/lib/docker:/sbin/nologin
saslauth:x:994:76:Saslauthd user:/run/saslauthd:/sbin/nologin
mysql:x:1000:1000::/home/mysql:/sbin/nologin
www:x:1001:1001::/home/www:/sbin/nologin
nscd:x:28:28:NSCD Daemon:/:/sbin/nologin
named:x:25:25:Named:/var/named:/sbin/nologin
```

##### 13. 几个花活
```
#从文件中找出长度大于80的行
awk 'length>80' netstat.txt

#按连接数查看客户端IP
netstat -ntu | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr

#打印99乘法表
seq 9 | sed 'H;g' | awk -v RS='' '{for(i=1;i<=NF;i++)printf("%dx%d=%d%s", i, NR, i*NR, i==NR?"\n":"\t")}'
```
