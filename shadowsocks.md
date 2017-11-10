## 安装配置shadowsocks服务器端
1. 安装
```
yum install epel-release -y
yum install gcc gettext autoconf libtool automake make openssl-devel pcre-devel asciidoc xmlto zlib-devel openssl-devel libsodium-devel udns-devel libev-devel -y
wget https://copr.fedorainfracloud.org/coprs/librehat/shadowsocks/repo/epel-7/librehat-shadowsocks-epel-7.repo
cp librehat-shadowsocks-epel-7.repo /etc/yum.repos.d/
yum update
yum install shadowsocks-libev
```
2. 配置
```
vi /etc/shadowsocks-libev/config.json

{
    "server":"0.0.0.0",
    "server_port":8388,
    "password":"password",
    "method":"aes-256-cfb",
    "mode":"tcp_and_udp"
}
```
参数说明：
* server       服务器监听的地址
* server_port  服务端监听的端口，默认8388
* local_address   本地监听端口
* local_port  本地监听端口
* password    用以加密的密匙，链接时使用
* timeout     超时时间（秒）
* method      加密方法，我们使用的aes-256-cfb
* fast_open   是否启用TCP_Fast_Open
* workers     worker数量

多用户配置
```
{
    "server":"your_server_ip",
    "local_address": "127.0.0.1",
    "local_port":1080,
    "port_password":{
         "8989":"password0",
         "9001":"password1",
         "9002":"password2",
         "9003":"password3",
         "9004":"password4"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

3. 运行
```
/usr/bin/ss-server -c /etc/shadowsocks-libev/config.json
```
4. 添加到服务
```
vi /usr/lib/systemd/system/shadowsocks.service

[Unit]
Description=Shadowsocks Server
Documentation=https://github.com/shadowsocks/shadowsocks
After=network.target
[Service]
Type=simple
User=nobody
ExecStart=/usr/bin/ss-server -c /etc/shadowsocks-libev/config.json
ExecReload=/bin/kill -HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true
KillMode=process
[Install]
WantedBy=multi-user.target

systemctl start shadowsocks
systemctl enable shadowsocks
systemctl stop shadowsocks
```
5. 查看日志
```
journalctl | grep ss-server
```

## 安装shadowsocks客户端
1. 下载解压
2. 安装运行（windows需要安装.net 4.0)，依据服务器端的配置填写参数
3. 浏览器安装switchSharp插件配置http代理


