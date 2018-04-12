## pscp用途
实现linux服务与windows通过ssh进行文件传输

## pscp用法
1. 下载pscp.exe放入windows/system32目录，打开dos，查看帮助
```
pscp --help
```
2. 注意事项，所有options参数在前，source 和target在后，顺序颠倒会报错
3. 如果用的是ssh密钥的方式连接，可用-i加上密钥地址连接，不过要注意，如果密钥
格式不是putty的格式会报错
```
Unable to use key file "C:\Users\yliu.abcft\.ssh\id_rsa" (OpenSSH SSH-2 private key (old PEM format))
Fatal: Disconnected: No supported authentication methods available (server sent: publickey,gssapi-keyex,gssapi-with-mic)
```
可用下载puttygen导入密钥转换为putty格式

4. 命令示例
```
pscp -C -P 9527  -i C:\Users\yliu.abcft\.ssh\putty-private.ppk yliu@120.26.90.111:/home/yliu/raa36.log  E:\Download
```
