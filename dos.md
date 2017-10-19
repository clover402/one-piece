## 设置DOS窗口的宽高
1. 运行 -> cmd -> dos窗口上沿点右键 -> 属性 -> 布局
2. 设置缓冲器大小和屏幕大小，最好保持宽度一致

## DOS中复制
右键选标记 -> 选中文字 -> 回车完成复制

## 批处理隐藏DOS窗口
1. 新建vbs后缀文件
2. 编辑该文件输入
```
createobject("wscript.shell").run "d:\your\file\path.bat",0
```

## 无法使用dir命令
1. 选择窗口标题，点击右键，选属性
2. 字体 -> 字体选consolas 保存退出


## netstat基本用法
查看网络连接命令
* -a 查看所有连接和监听端口
* -n 以数字形式显示所有地址和端口
* -o 显示每个连接的进程id
* -p 指定要显示的协议，协议包含: TCP, UDP, TCPv6, or UDPv6.

## findstr
在命令中的结果中查找字符串如：
```
netstat -ano | findstr ":8983"
```
