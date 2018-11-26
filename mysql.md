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

## windows启动mysql5.7
1. 下载解压
2. 在解压目录新建data目录
3. 新建my.ini放入bin目录
4. 以admin权限运行如下命令
```
mysqld --initialize --user=mysql --console //注意有个临时密码， 要记下来
mysqld -install
net start MySQL
```


## mysqladmin
此命令可以直接设置密码及修改密码
```
mysqladmin -u root PASSWORD xxx
mysqladmin -u root -p password xxx
```
导入数据库
```
mysqladmin -uroot -p dbname < dbfile.sql
mysql -uroot -p -e "sql command" > filename.txt
```

## mysqldump
数据库导入导出
```
mysqldump -uroot -p dbname > file.txt

```

## 创建用户及授权【create/select/update/delete/insert】
```
grant all privileges on dbname.* to user@localhost identified by password
grant all privileges on dbname.* to user@'10.1.1.2' identified by password

```

## mysql命令

```
show tables like 'v\_%'; --搜索以v开头的视图
show fields form tbname; --展示列信息
truncate table tbname; --清空表效率高，但删除的数据不可恢复
explain select * from tbname where id=xxx; --查看索引使用情况
alter table tbname ENGINE=MyISAM/InnoDB; --设置搜索引擎
create view viewname(col1,...) as select语句 with check option; --创建视图
create or replace view ...; --修改视图
show function status \G --查看创建的函数
show create procedure \G --存储过程
show triggers \G --查看触发器
load data infile $filename into table $tbname [option];
--导入文件【路径最好用/】
select * into outfile $filname [option] from $tbname;
--导出文件
source $filname --执行sql文件
tee $filename --将结果保存到文件

foreign key foreignFieldName references targetTb(fieldName);
--外键定义
constraint 约束名 unique(field1, fiedl2); --创建唯一约束
alter table tbname add/drop constraint-sql; --给存在的表加/删约束
CONSTRAINT 约束名 CHECK(约束条件); --创建check约束
IFNULL(expression,value)
ENCODE(str,pass_str)   --加密
UUID()
MD5()
SHA1()
IN ANY ALL EXISTS【判断子查询是否存在】 --子查询
```

## 索引
```
create index indexName on tbname(fieldname); --创建索引
```
** 无法使用索引的情况：**
1. LIKE '%w%', LIKE '%w'
2. 使用了 IS NOT NULL, <>
3. 使用运算/函数
4. 复合索引的第一列不在where条件中



## mysql通配符
%代表0及以上的字符
_代表一个字符

## limit要与order by同时使用，因为没有指定排序时数据是随机排序

## \G 替换; 可以更有条理的展示内容

## 三范式
1NF：数据库表的每一列都是不可分割的基本数据项，同一列中不能有多个值，即实体中的某个属性不能有多个值或者不能有重复的属性。
2NF: 属性完全依赖于主键
3NF: 一个数据库表中不包含已在其它表中已包含的非主关键字信息。属性不依赖于其它非主属性。也就是说，如果存在非主属性对于码的传递函数依赖，则不符合3NF的要求

## 数据库建议
1. 性能方面： char > varchar > text
2. where不要使用1=1 会导致无法使用索引，影响性能
3. DISTINCT是对整个结果集进行数据重复抑制的，而不是针对每一个列

