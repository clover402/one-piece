# com.alibaba.fastjson学习

## JSONObject
类似于Map的一种数据结构
### 构造函数
#### JSONObject(Map<String, Object> map):指定map
#### JSONObject(int initialCapacity)：指定map大小
#### JSONObject(int initialCapacity, boolean ordered):指定是否要排序能力
### 方法
#### JSONObject getJSONObject(String key)：返回键值对应的内容并转化为JSONObject对象
#### JSONArray getJSONArray(String key):返回键值对应的内容并转化为JSONArray对象
#### <T> T getObject(String key, Class<T> clazz)
#### <T> T getObject(String key, Type type)
#### <T> T getObject(String key, TypeReference typeReference):获取键值对应的内容并解析成指定类型额对象
#### <T> T toJavaObject(Class<T> clazz)：转化为指定类
#### String toString()/toJSONString():转字符串  
#### 还有各种get，主要是基本类型及其包装类，String，Date，BigDeciaml，BigInteger  
### 静态方法
#### 各种parse：转具体对象
#### 各种toJSONString/toJSONBytes：转字符串/字节码
#### 各种toJSON:转Object对象  
#### isValid/isValidObject/isValidArray:可以提交判断防止报异常

## JSONArray
类似List的数据结构
### 构造函数
#### JSONArray(List<Object> list)
#### JSONArray(int initialCapacity)：指定长度  
### 方法
#### JSONObject getJSONObject(int index)：与JSONObject类似，只是通过index来取值
#### JSONArray getJSONArray(int index)  
#### <T> T getObject(int index, Class<T> clazz)  
#### <T> List<T> toJavaList(Class<T> clazz)：转化成精确的List类  
#### 类似JSONObject，各种基本类型包装类等的get  

## JSONPObject
主要属性function,parameters  
为了使用jsonp解决跨越问题

## PropertyNamingStrategy
小工具，可以把属性转换成不同的命名风格：CamelCase[驼峰], SnakeCase[蛇形], PascalCase[大驼峰], KebabCase[短横线命名]  
还有一种匈牙利命名（开头字母用变量类型的缩写，其余部分用变量的英文或英文的缩写，要求单词第一个字母大写。），这里没有。
