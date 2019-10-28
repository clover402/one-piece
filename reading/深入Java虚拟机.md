## 典型配置举例
### 并行收集器
#### 吞吐量优先
```
java -Xmx3800m -Xms3800m -Xmn2g -Xss128k -XX:+UseParallelGC -XX:ParallelGCThreads=20
java -Xmx3550m -Xms3550m -Xmn2g -Xss128k -XX:+UseParallelGC -XX:ParallelGCThreads=20 -XX:+UseParallelOldGC
java -Xmx3550m -Xms3550m -Xmn2g -Xss128k -XX:+UseParallelGC -XX:MaxGCPauseMillis=100 -XX:+UseAdaptiveSizePolicy
```  
* -Xmx3550m: 设置JVM最大可用内存为3550M
* -Xms3550m: 设置JVM初始内存为3550m，此值建议设置与-Xmx相同，以避免每次垃圾回收完成后JVM重新分配内存
* -Xmn2g：设置年轻代大小为2G。整个堆大小=年轻代大小 + 年老代大小 + 持久代大小。持久代一般固定大小为64m，所以增大年轻代后，将会减小年老代大小。此值对系统性能影响较大，Sun官方推荐配置为整个堆的3/8。
* -Xss128k：设置每个线程的堆栈大小.操作系统对一个进程内的线程数还是有限制的，不能无限生成，经验值在3000~5000左右
* **ParallelGCThreads**:并行收集器线程数
* **UseParallelGC**:选择垃圾收集器为并行收集器.此配置仅对年轻代有效。即上述配置下，年轻代使用并发收集，而年老代仍旧使用串行收集
* **UseParallelOldGC**:配置年老代垃圾收集方式为并行收集
* **MaxGcPauseMillis**:设置每次年轻代垃圾回收的最长时间
* **UseAdaptiveSizePolicy**:设置此选项后，并行收集器会自动选择年轻代区大小和相应的Survivor区比例，以达到目标系统规定的最低相应时间或者收集频率等，此值建议使用并行收集器时一直打开。
