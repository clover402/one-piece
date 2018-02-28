## SpringCloud
1. 启动
mvnw.cmd spring-boot:run

2. 打包执行
mvnw.cmd package
java -jar target/myproject-0.0.1-SNAPSHOT.jar


## SpringCloud POM:
1. **父亲parent**
```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.3.1.RELEASE</version>
</parent>
```
2. **依赖dependency**
```
启动：
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

监控：
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```
3. **插件plugin**
```
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
```
