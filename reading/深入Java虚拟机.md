## JAVA虚拟机的构成
### 机制：类装载子系统，运行引擎
### 构成
#### 栈
1. 局部变量区

#### 堆
#### 方法区
#### 程序计数器
线程中正在被执行的指令地址
#### 本地方法栈


## 垃圾收集算法
1. 标记-清除算法：效率低，产生大量碎片
2. 复制：内存分两块，每次只用一半，当半块用完，把存活的对象复制到另外一半上，然后把原来的一次全部清除
3. 分代收集：根据对象存活周期划分几个块，然后依次执行复制的算法来回收垃圾

## 判断对象死亡的方法
1. 引用计数算法(Reference Counting)：无法解决循环引用的问题
2. 根搜索算法(GC Roots Tracing)：当一个对象到GC Roots没有引用链相连，则证明对象不可用。GC Roots包括：
* VM栈中的引用
* 方法区的静态引用
* JNI（Native方法）中的引用

## 垃圾收集器
1. Serial收集器：单线程
2. ParNew收集器：多线程
3. Parallel Scavenge收集器：多线程，大吞吐量长时间STW
4. Serial Old：年老代，单线程
5. Parallel Old： 年老代，吞吐量优先
6. CMS（Concurrent Mark Sweep）：最短停顿时间，高实时高交互


## 分代示意图
![分代](http://m.qpic.cn/psb?/V14Yvw6F0uSJqd/88Bg7V2KIZHW2AtSsSqt*TqZu8Ky8rgGoAFKZghpUCg!/b/dFIBAAAAAAAA&bo=bgP9AQAAAAADB7M!&rf=viewer_4)

## 典型配置举例
### 基础配置
* -Xmx3550m: 设置JVM最大可用内存为3550M
* -Xms3550m: 设置JVM初始内存为3550m，此值建议设置与-Xmx相同，以避免每次垃圾回收完成后JVM重新分配内存
* -Xmn2g：设置年轻代大小为2G。整个堆大小=年轻代大小 + 年老代大小 + 持久代大小。持久代一般固定大小为64m，所以增大年轻代后，将会减小年老代大小。此值对系统性能影响较大，Sun官方推荐配置为整个堆的3/8。
* -Xss128k：设置每个线程的堆栈大小.操作系统对一个进程内的线程数还是有限制的，不能无限生成，经验值在3000~5000左右
* NewRatio: 设置年轻代（包括Eden和两个Survivor区）与年老代的比值（除去持久代）
* SurvivorRatio:设置年轻代中Eden区与Survivor区的大小比值
* MaxPermSize: 设置持久代大小
### 吞吐量优先并行收集器
```
java -Xmx3800m -Xms3800m -Xmn2g -Xss128k -XX:+UseParallelGC -XX:ParallelGCThreads=20
java -Xmx3550m -Xms3550m -Xmn2g -Xss128k -XX:+UseParallelGC -XX:ParallelGCThreads=20 -XX:+UseParallelOldGC
java -Xmx3550m -Xms3550m -Xmn2g -Xss128k -XX:+UseParallelGC -XX:MaxGCPauseMillis=100 -XX:+UseAdaptiveSizePolicy
```  
* **ParallelGCThreads**:并行收集器线程数
* **+UseParallelGC**:选择垃圾收集器为并行收集器.此配置仅对年轻代有效。即上述配置下，年轻代使用并发收集，而年老代仍旧使用串行收集
* **+UseParallelOldGC**:配置年老代垃圾收集方式为并行收集
* **MaxGcPauseMillis**:设置每次年轻代垃圾回收的最长时间
* **+UseAdaptiveSizePolicy**:设置此选项后，并行收集器会自动选择年轻代区大小和相应的Survivor区比例，以达到目标系统规定的最低相应时间或者收集频率等，此值建议使用并行收集器时一直打开。

### 响应时间优先的并发收集器
```
java -Xmx3550m -Xms3550m -Xmn2g -Xss128k -XX:ParallelGCThreads=20 -XX:+UseConcMarkSweepGC -XX:+UseParNewGC
java -Xmx3550m -Xms3550m -Xmn2g -Xss128k -XX:+UseConcMarkSweepGC -XX:CMSFullGCsBeforeCompaction=5 -XX:+UseCMSCompactAtFullCollection

```
* **UseConcMarkSweepGC**: 设置年老代为并发收集。
* **UseParNewGC**: 设置年轻代为并行收集。
* **CMSFullGCsBeforeCompaction**: 此值设置运行多少次GC后对内存空间进行压缩、整理
* **UseCMSCompactAtFullCollection**: 打开对年老代的压缩，可能会影响性能，但可以消除碎片

### 辅助信息
* **+PrintGC**: 打印GC相关的调试信息
* **+PrintGCDetails**: 上面的加强版本，更多信息
* **+PrintGCTimeStamps**： 加时间戳，与上面2个混合使用
* **+PrintGCApplicationConcurrentTime**: 打印每次垃圾回收前程序未中断的执行时间
* **+PrintHeapAtGC**: 打印前后的详细堆栈信息
* **-Xloggc:filename:**:与上面配合使用，记录日志信息到文件

## JVM调优工具
### jvisualVM
jdk包里面都有，在bin目录下面，可以选择安装一些插件帮助分析  
主要使用：监视、线程、visual GC（插件）  


## Java内存模型
Java内存模型规定JVM有主内存，主内存是多个线程共享；而每个线程都有自己的工作内存，工作内存存储了主内存的某些对象的副本，大小有限制。  
当线程操作某个对象时，执行顺序如下:  
1. 从主存复制变量到当前工作内存（read and load）
2. 执行代码，改变共享变量值（use and assign）
3. 用工作内存数据刷新主存相关内容（store and write）
### 可见性  
一个线程修改了共享变量的值，其他线程都能看到修改后的值
### 有序性
read,load,use的顺序可以由JVM实现系统决定。多线程时如果顺序设置不合理会导致变量的值不合理
### synchronized关键字
每个锁对象有两个队列，一个就绪队列，一个阻塞队列。就绪队列存储了将要获得锁的线程，阻塞队列存储了被阻塞的线程，当他被唤醒（notify）后才会进化就绪队列。
1. 对象的方法加上synchronized修饰符，实际上就是执行该方法是对其象执行锁操作
2. synchronized(被锁的对象){ 锁定期间执行的代码 }
### volatile关键字
该变量只能保证多线程的可见性，不能保证有序性。任何被volatile修饰的变量，都不拷贝副本到工作内存，任何修改都及时写在主内存.  
要使volatile变量提高理想的线程安全，必须同时满足下面2个条件：
1. 对变量的写操作不依赖当前值
2. 变量没有包含在具有其他变量的不变式中  
volatile适合这种场景：一个变量被多个线程共享，线程直接给这个变量赋值。这是一种很简单的同步场景，这时候使用volatile的开销将会非常小。




