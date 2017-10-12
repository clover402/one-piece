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
