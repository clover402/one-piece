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
