## Windows安装docker
1. 安装windows版docker客户端
2. 安装Docker Toolbox管理工具和ISO镜像
3. 安装Oracle VM Virtualbox
4. 安装git msys-git unix工具
5. 运行docker run hello-world测试

## 运行在线镜像
1. 在官网搜索https://hub.docker.com/
2. 运行镜像
```
docker run $image_name
```
该命令会先查找本地是否有这个镜像，如果不存在就会自动从hub上获取
3. 查看镜像
```
docker images
```

## 创建docker镜像
1. 创建Dockerfile
2. 终端运行下面命令
```
docker build -t docker-edx .
```
3. 查看镜像
```
docker images
```
4. 运行镜像
```
docker run docker-edx
```
