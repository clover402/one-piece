#1. ps
ps  -eo pid,%cpu,%mem,vsz,rss,lstart,cmd,cputime,stat  | grep php-fpm

#2. php-fpm



#3. 统计代码行数
```
find . -name "*.java" |xargs cat|wc -l
find . -name "*.java" |xargs cat|grep -v ^$|wc -l #过滤空行
```

#4. 后台运行脚本
```
./test.sh &
```
#5. 常用命令
```
cut -c 8- -d':'
sort -rn -k1
uniq -dc
tr '[a-z]' '[A-Z]'
tr -d '\r'
grep -c
grep -v
grep -n -i
```

#5. 查看进程
```
ps -afxo user,ppid,pid,pgid,command
ps axjf
ps aux
pstree -up
```
#6. 修改进程优先级
```
nice -n -5 vim &
renice -5 pid
cat /proc/cpuinfo |grep "physical id"|sort |uniq|wc -l #查看物理CPU的个数
cat /proc/cpuinfo |grep "physical id"|grep "0"|wc -l #每个cpu的核心数
lastlog last # 查看二进制文件
```
#7. 日志相关命令
`rsyslog
logger
logrotate
newsyslog`

#8. sed

```
sed -i 's/abc/123/g' test
sed ":a;N;s/\n/\r\n/g;ta" a.txt
sed ":a;N;s/\n/\r\n/g;$!ba" a.txt
```
a和ta是配套使用，实现跳转功能。t是test测试的意思
N是把下一行加入到当前的hold space模式空间里，使之进行后续处理
sed是按行处理文本数据的，每次处理一行数据后，都会在行尾自动添加trailing newline，其实就是行的分隔符即换行符
```
sed 'H;$!d;${x;s/^\n//;s/\n/,/g}' text
```
1,11,2,11,22,111,222
上面的命令执行过程是这样的，使用H将每一行都追加到保持空间，这里利用d命令打断常规的命令执行流程，让sed继续读入新的一行，直接到将最后一行都放到保持空间。这个时候使用x命令将保持空间的内容交换到模式空间，模式空间的内容现在是这样的：\n1\n11\n2\n11\n22\n111\n222  替换的步骤分成两个，首先去掉首个回车符，然后把剩余的回车符替换成逗号。


#9. awk
```
awk '{if(NR==4){print $1,$2,$3}else{print}}' test
```
NR与OFS这两个是awk内建的变量，NR表示当前读入的记录数，你可以简单的理解为当前处理的行数，OFS表示输出时的字段分隔符，默认为空格.
$N其中N为相应的字段号，这也是awk的内建变量，它表示引用相应的字段，因为我们这里第一行只有三个字段，所以只引用到了$3。除此之外另一个这里没有出现的$0，它表示引用当前记录（当前行）的全部内容

- FS 字段分隔符
- RS 记录分隔符
- OFS 输出字段分隔符
- ORS 输出记录分隔符
- NF 当前记录字段数
- NR 已读入记录数
- FNR 文件记录数


```
awk -F'.' '{if(NR==2){print $1 "\t" $2 "\t" $3}}' test
awk 'BEGIN{FS=".";OFS="\t"}{if(NR==2){print $1, $2, $3}}' test
```


#10. netstat
```
netstat -tunlp | grep xxx
```


#11. ssh tunnel socket
```
ssh -TnN -D 1080 -o TCPKeepAlive=yes -o ServerAliveInterval=30 git@47.56.114.34 "vmstat 30"

```
