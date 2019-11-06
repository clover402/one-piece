## Java虚拟机
### 运行时数据区域
![运行时数据区域](http://m.qpic.cn/psb?/V14Yvw6F0uSJqd/cj1iDWYAfGy3*QDjiH1UObSR4fDrVGr.g8BmIjRNPoI!/b/dEEBAAAAAAAA&bo=cQJtAQAAAAADBz0!&rf=viewer_4)
#### 程序计数器
一块较小的内存空间，可以看做当前线程所执行的字节码的行号指示器。每个线程都有一个独立的程序计数器。如果执行的是Native方法则计数器的值为空。
#### Java虚拟机栈
它的生命周期与线程相同  
每个方法被执行的时候都会同时创建一个栈帧（Stack Frame）用于存储局部变量表、操作栈、动态链接、方法出口等。
