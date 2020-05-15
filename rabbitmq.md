# Windows下安装运行rabbitmq
## 1. 安装Erlang
1. 下载地址：https://www.erlang.org/downloads
2. 安装好后新建环境变量ERLANG_HOME--安装路径（eg:D:\Program Files\erl10.0.1）  
3. 配置path环境变量，增加erlang的bin目录到path（eg:%ERLANG_HOME%\bin)  
4. 测试：打开cmd，输入erl，正常反应则成功
## 2.安装rabbitmq
1. 下载安装  
* exe安装地址：http://www.rabbitmq.com/install-windows.html
* 解压缩安装地址：http://www.rabbitmq.com/install-windows-manual.html  
2. 配置环境变量  
* RABBITMQ_SERVER  -> 安装路径（eg:D:\Program Files\rabbitmq_server-3.7.7)  
* path -> 增加sbin目录（eg:%RABBITMQ_SERVER%\sbin)  
3. 启动管理界面：cmd-> rabbitmq-plugins enable rabbitmq_management  
4. 启动服务：rabbitmq-server
5. 测试：本地浏览器打开http://127.0.0.1:15672, 然后用guest/guest可登录
