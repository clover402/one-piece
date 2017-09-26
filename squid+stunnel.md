## 代理服务器搭建
### 服务器端-安装squid
1. 安装squid及openssl
```
yum install squid openssl openssl-devel
```
2. 生成加密代理证书
```
# cd /etc/squid 
# openssl req -new > tank.csr //要求输入密码和确认密码及其他一些证书相关信息
# openssl rsa -in privkey.pem -out tank.key //输入上面输入的密码 
# openssl x509 -in tank.csr -out tank.crt -req -signkey tank.key -days 3650 
```
3. 配置squid
```
vim /etc/squid/squid.conf 

acl OverConnLimit maxconn 10 //限制每个IP最大允许10个连接，防止攻击 
minimum_object_size 1 KB //允午最小文件请求体大小 
maximum_object_size 1 MB //允午最大文件请求体大小 
cache_swap_low 10 //最小允许使用swap 10% 
cache_swap_high 25 //最大允许使用swap 25% 
cache_mem 64 MB //可使用内存 

/*****************上面是新增，下面是修改************************/ 

cache_dir ufs /var/spool/squid 2048 16 256 //2048存储空间大小，一级目录16个，二级256个 
https_port 4430 cert=/etc/squid/tank.crt key=/etc/squid/tank.key //端口可自定义 
http_access allow all 
```
4. 启动squid
```
service squid start
```
注： 如果有防火墙或安全组，需要开通对应的入端口，此处为4430




### 客户端-安装stunnel
1. 下载stunnel并安装
2. 将之前生成的证书tank.crt和私钥privkey.pem放到config文件夹
3. 修改配置文件
```
debug = 7
output = ./stunnel.log

cert = tank.crt
key = privkey.pem

client = yes

[https]
accept  = 127.0.0.1:8080
connect = 47.88.55.172:4430
TIMEOUTclose = 0
```
4. 启动服务器，修改浏览器代理为127.0.0.1:8080

### 查看日志
1. 可以查看日志文件/var/log/squid/access.log来访问记录和错误信息
2. 解读日志
```
1206507660.803  84367 192.168.1.114 TCP_MISS/502 1486 GET http://123.138.238.114/QQ2008SpringKB1.exe - DIRECT/123.138.238.114 text/html
```
默认包含10个字段
1. 时间戳: 请求完成时间，以 Unix 时间来记录的（UTC 1970-01-01 00:00:00 开始的时间）它是毫秒级的。 squid使用这种格式而不是人工可读的时间格式，是为了简化某些日志处理程序的工作。
2. 响应时间: 对HTTP响应来说，该域表明squid花了多少时间来处理请求。在squid接受到HTTP请求时开始计时，在响应完全送出后计时终止。响应时间是毫秒级的。尽管时间值是毫秒级的，但是精度可能是10毫秒。在squid负载繁重时，计时变得没那么精确。
3. 客户端地址: 该域包含客户端的IP地址，或者是主机名.
4. 结果/状态码: 该域包含2个 token，以斜杠分隔。第一个token叫结果码，它把协议和响应结果（例如TCP_HIT或UDP_DENIED）进行归类。这些是squid专有的编码，以TCP_开头的编码指HTTP请求，以UDP_开头的编码指ICP查询。第2个token是HTTP响应状态码（例如200,304,404等）。状态码通常来自原始服务器。在某些情形下，squid可能有义务自己选择状态码.
5. 传输size: 该域指明传给客户端的字节数。严格的讲，它是squid告诉TCP/IP协议栈去发送给客户端的字节数。这就是说，它不包括TCP/IP头部的overhead。也请注意，传输size正常来说大于响应的Content-Length。传输size包括了HTTP响应头部，然而Content- Length不包括。
6. 请求方式: 该域包含请求方式.
7. URI: 该域包含来自客户端请求的URI。大多数记录下来的URI实际是URL（例如，它们有主机名）。在记日志时，squid删掉了在第一个问号(?)之后的所有URI字符，8除非禁用了strip_query_terms指令。
8. 客户端身份: 无
9. 对端编码/对端主机: 对端信息包含了2个token，以斜杠分隔。它仅仅与cache 不命中的请求有关。第一个token指示如何选择下一跳，第二个token是下一跳的地址。当squid发送一个请求到邻居cache时，对端主机地址是邻居的主机名。假如请求是直接送到原始服务器的，则squid会写成原始服务器的IP地址或主机名–假如禁用了log_ip_on_direct。 NONE/-这个值指明squid不转发该请求到任何其他服务器。
10. 内容类型: 原始access.log的默认的最后一个域，是HTTP响应的内容类型。 squid从响应的Content-Type头部获取内容类型值。假如该头部丢失了，squid使用一个横杠(-)代替。

假如激活了 log_mime_hdrs 指令，squid在每行追加2个附加的域

11. HTTP请求头部: Squid 编码HTTP请求头部，并且在一对方括号之​​间打印它们。方括号是必须的，因为squid不编码空格字符。编码方案稍许奇怪。回车（ASCII 13）和换行（ASCII 10）分别打印成\r和\n。其他不可打印的字符以RFC 1738风格来编码，例如Tab（ASCII 9）变成了%09。
12. HTTP响应头部: Squid编码HTTP响应头部，并且在一对方括号之​​间打印它们。注意这些是发往客户端的头部，可能不同于从原始服务器接受到的头部。
