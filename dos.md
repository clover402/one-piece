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
