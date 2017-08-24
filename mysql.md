## 忘记密码
### v5.7以前版本
```
[root@localhost ~]# service mysqld stop
[root@localhost ~]# mysqld_safe --skip-grant-tabels& #绕过授权表启动服务
[root@localhost ~]# mysql -u root
mysql> use mysql;
mysql> update user set password=password("new_pass") where user="root";
mysql> flush privileges;
mysql> quit;
[root@localhost ~]# service mysqld restart
```
### v5.7及以后版本
```
[root@localhost ~]# service mysqld stop
[root@localhost ~]# vim /etc/my.cnf
-------------增加如下参数---------
[mysqld]
skip-grant-tables
-------------wq保存退出-----------
[root@localhost ~]# service mysqld start
[root@localhost ~]# mysql -u root
mysql> use mysql;
mysql> update user set authentication_string=password("new_pass") where user="root";
mysql> flush privileges;
mysql> quit;
[root@localhost ~]# service mysqld restart
```
当你登陆mysql之后你会发现，当你执行命令时会出现
ERROR 1820 (HY000): You must reset your password using ALTER USER statement;
这是提示你需要修改密码 当你执行了
```
mysql> SET PASSWORD = PASSWORD('root');
```
如出现ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
你需要执行两个参数来把mysql默认的密码强度的取消了才行 当然也可以把你的密码复杂度提高也行啊
```
mysql> set global validate_password_policy=0; 
mysql> set global validate_password_mixed_case_count=2;
mysql> SET PASSWORD = PASSWORD('root');
```
