# com.alibaba.fastjson学习

## JSONObject
类似于Map的一种数据结构
### 构造函数
#### JSONObject(Map<String, Object> map)
#### JSONObject(int initialCapacity)

## JSONArray

## JSONPObject
主要属性function,parameters  
为了使用jsonp解决跨越问题

## PropertyNamingStrategy
小工具，可以把属性转换成不同的命名风格：CamelCase[驼峰], SnakeCase[蛇形], PascalCase[大驼峰], KebabCase[短横线命名]  
还有一种匈牙利命名（开头字母用变量类型的缩写，其余部分用变量的英文或英文的缩写，要求单词第一个字母大写。），这里没有。
