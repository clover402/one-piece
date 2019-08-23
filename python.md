##windows安装扩展
```
pip install 路径\文件名.whl
```

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
* os.chdir(path) #改变当前工作目录
* os.fork() #创建子进程，子进程从创建后开始执行


## sys模块
* sys.exit() 引发一个 SystemExit异常，若没有捕获这个异常，Python解释器会直接退出；捕获这个异常可以做一些额外的清理工作。0为正常退出，其他数值（1-127）为不正常，可抛异常事件供捕获。
* sys.exc_info() 获取异常的详细信息(异常类型，异常对象，traceback)


## 异常处理
1. 捕获处理异常try...except
```
try:
    xxxx
except XXError, e:
    xxxx
else:
    xxxx
```
2. 抛出异常
raise  XXError

## traceback模块
traceback.format_tb(tb) return a list of strings ready for printing

## \_\_name\_\_ 与 \_\_main\_\_
如果直接执行脚本则__name__值为__main__,表示入口
如果把文件作为module引入，则__name__的值为模块名

## \_\_init\_\_.py文件
将多个.py文件组织起来，以便在外部统一调用，和在内部互相调用.
有时在import语句中会出现通配符*，导入某个module中的所有元素，这就是在__init__.py中写入
```
__all__ = ['module_1', 'module_2', 'modul3_x']
```
导入整个包时实际上是执行了包里的__init__文件

## 函数参数*
定义函数参数时如果前有一个* 表示接受元祖（tuple）参数
定义函数参数时如果前有两个* 表示接受字典（dict）参数

## multiprocessing模块
Python提供了非常好用的多进程包multiprocessing，只需要定义一个函数，Python会完成其他所有事情。借助这个包，可以轻松完成从单进程到并发执行的转换。multiprocessing支持子进程、通信和共享数据、执行不同形式的同步，提供了Process、Queue、Pipe、Lock等组件
* **Process([group [, target [, name [, args [, kwargs]]]]])** 创建进程的类：target表示调用对象，args表示调用对象的位置参数元组。kwargs表示调用对象的字典。name为别名。group实质上不使用。方法：is_alive()、join([timeout])、run()、start()、terminate()。其中，Process以start()启动某个进程。

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


## pip2报错unknown encoding: cp65001
set PYTHONIOENCODING=UTF-8 然后再运行

## pip下载报错
修改镜像
pip install flask -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com

##
