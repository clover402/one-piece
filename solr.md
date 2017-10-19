## 部署运行solr4.9
1. 安装最新版的java（JDK），并配置好环境变量PATH,JAVA_HOME,CLASSPATH
2. 官网下载solr4.9.zip,并解压
3. cmd今入example目录，运行如下命令启动solr
```
cd solr4.9/example
java -jar start.jar
```
4. 添加文档
```
cd exampledocs
java -jar post.jar *.xml
```
5. 浏览器输入http://localhost:8983/solr/ 进入管理页面


## 浏览查看数据
1. 打开地址http://localhost:8983/solr/core_name/browse


## solr7.0.1
### 启动服务
```
bin\solr.cmd start -V -f #启动服务
bin\solr.cmd create -c clover #创建core
```
### 导入数据
```
cd example\exampledocs\
java -Dtype=text/csv -Durl=http://localhost:8983/solr/core_name/update  -jar post.jar   books.csv
```
