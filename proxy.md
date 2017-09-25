## 代理服务器搭建
### 服务器端-安装squid

### 服务器端-安装stunnel

### 客户端-安装stunnel
1. 下载stunnel并安装
2. 将之前生成的cert证书和私钥放到config文件夹
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
