## **检测不到网卡**
1. 打开设备管理器
2. 查看/显示隐藏设备
3. 非即插即用驱动程序
4. NetGroup Packet Filter Driver 右键属性 -> 驱动程序 -> 启动类型修改类型为“系统” -> 启动，
5. 运行wireshark，网卡可以正常检测到了
6. 如还不行可以cmd -> net start npf (打开网络抓包服务)

## **捕获过滤器**
* 语法：&lt;Protocol&gt; &lt;Direction&gt; &lt;Hosts&gt; &lt;Value&gt; &lt;Logic Operations&gt;
    * Protocol: 1
    * Direction: 2
    * Hosts: 3
    * Value: 4
    * Logic Operations: 5
