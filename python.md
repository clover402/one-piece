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

## logging模块

## os.path模块
* os.path.expanduser(path)  #把path中包含的"~"和"~user"转换成用户目录
* os.path.isdir(path)  #判断路径是否为目录
* os.path.join(path1[, path2[, ...]])  #把目录和文件名合成一个路径

## os模块
* os.makedirs(path[, mode])  #递归创建目录
* os.mkdir(path[, mode])  #以数字mode的mode创建一个名为path的文件夹.默认的 mode 是 0777 (八进制)。

## atexit模块
只定义了一个register函数用于注册程序退出时的回调函数，我们可以在这个回调函数中做一些资源清理的操作。
注：如果程序是非正常crash，或者通过os._exit()退出，注册的回调函数将不会被调用。
查阅atexit的源码，你会发现原来它内部是通过sys.exitfunc来实现的，它先把注册的回调函数放到一个列表中，当程序退出的时候，按先进后出的顺序调用注册的回调。如果回调函数在执行过程中抛出了异常，atexit会打印异常的文字信息，并继续执行下一下回调，直到所有的回调都执行完毕，它会重新抛出最后接收到的异常。

使用实例：
```
import atexit

def exit0(*args, **kwarg):
    print 'exit0'
    for arg in args:
        print ' ' * 4, arg

    for item in kwarg.items():
        print ' ' * 4, item

def exit1():
    print 'exit1'
    raise Exception, 'exit1'

def exit2():
    print 'exit2'

atexit.register(exit0, *[1, 2, 3], **{ "a": 1, "b": 2, })
atexit.register(exit1)
atexit.register(exit2)

@atexit.register
def exit3():
    print 'exit3'

if __name__ == '__main__':
    pass
```
![结果](http://ww3.sinaimg.cn/mw690/7178f37ejw1esbukssjmjj20e90a5aax.jpg)
