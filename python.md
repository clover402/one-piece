## CGI编程
1. 安装apache程序
```
yum install httpd
```
2. 修改配置文件修改以下两处
```
vim /etc/httpd/conf/httpd.conf
```
```
<Directory "/var/www/cgi-bin">
   AllowOverride None
   Options +ExecCGI
   Order allow,deny
   Allow from all
</Directory>
AddHandler cgi-script .cgi .pl .py
```
3. 如此可在/var/www/cgi-bin里编写python脚本，在浏览器通过 *http://hostname/cgi-bin/yourfile.py* 来访问

4. python脚本第一行需要增加执行程序
```
#! /usr/bin/env python
```
5. 第一行要输出文件头部（且以换行结束）
```
print("Content-type:text/html\r\n")
```
6. 如此可正常访问，如果还有500错误，可以查看/var/logs/httpd/error.log找到错误原因并解决
