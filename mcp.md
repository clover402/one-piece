## MCP940工具包安装部署
1. 下载MCP940工具包并解压
```
cd d:/workspace/
mkdir mod_test #创建mod文件夹
mv mcp940.zip mod_test/mcp940.zip
unzip mcp940
```
2. 下载minecraft_server_1.12.jar,放入mod_test/jars目录
3. 下载minecraft_1.12客户端,放入~/AppData/Roaming目录并解压
4. 运行server端一次（需要修改mod_test\jars\eula.txt 改为yes）
```
java -Xmx1024M -Xms1024M -jar mod_test/jars/minecraft_server.1.12.2.jar nogui
```
5. 运行mod_test/mcpupdate.bat 升级mcp
6. 运行mod_test/decompile.bat 反编译，如果报编码错误可在该bat开头加上
```
set PYTHONIOENCODING=UTF-8
```
7. 下载并安装eclipse，复制mod_test/eclipse的完整路径，运行eclipse，以之前复制的地址打开workspace即可
