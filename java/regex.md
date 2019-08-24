# 正则表达式

一般讲解正则表达式都会按照技术的顺序去讲解，往往会比较枯燥，我决定换一个顺序，从使用场景来讲。

## 一、匹配字符串
&emsp;&emsp;一般使用Pattern.matches(String regex, CharSequence input)方法，该方法返回的是boolean值，这个方法会判断input字符串是否符合设定的regex正则表达式。  
```java
  String content = "I am noob from runoob.com.";
  String pattern = ".*runoob.*";
  boolean isMatch = Pattern.matches(pattern, content);
```  
&emsp;&emsp;下面进入正题讲下会用到的正则表达式的语法  

需求 | 正则表达式 |  说明  
-|-|-
数字 | \d 或 [0-9] | 匹配任意一个0-9的数字，说明\是转义符，\加字母可以表示很多特殊的含义。[xyz]表示字符集，匹配包含的任一字符 |
非数字 | \D 或 [^0-9] | 匹配任意一个不是0-9的字符，在[]中表示取反，与逻辑运算符！含义一样 |
字母 | [a-zA-Z] | [x-y]中-连接表示范围，目前支持的就3种a-z,A-Z,0-9,及其子集，比如0-5，a-m |
字类字符 | \w 或 [a-zA-Z0-9_] | 实际上就是C语言要求的变量名包含的字符范围，正则起源于Unix与C语言颇有渊源 |
非字类字符 | \W 或 [^a-zA-Z0-9_] | 发现没有它这里转义符有个规律，\跟小写字母是包含某些字符，\跟对应的大写是不包含某些字符 |
是否 | true&#124;false | &#124;用来分隔多个可选项 |
回车符 | \r | ASCII中为x0D，windows的换行由\r\n构成 |
换行符 | \n | ASCII中为x0A，linux的换行只有\n |
制表符 | \t | 相当于Tab键的效果 |
垂直制表符 | \v | 这个比较少见 |
空白字符 | \s 或 [ \f\n\r\t\v] | 包括空格、制表符、换页符（\f）等 |
非空字符 | \S 或 [^ \f\n\r\t\v] | 这个比较常用 |
任意字符 | [\S\s] | 正反合二为一就构成了整个世界 |
非换行字符 | . | 除了\r\n任意字符，比较常用 |
中国手机号 | [\d]{11} | 表示11位数字，后面的大括号中的数字表示前面内容出现的次数,这里是固定11次 |
大于99的整数 | [\d]{3,} | 表示3位及以上的数字 |
QQ号码  | ^[1-9]\d{4,10}$ | 表示数字出现4到10次 |
任意单词  | [a-zA-Z]+ | 通配符+表示一次或多次匹配前面的字符或子表达式（>=1） |
c语言变量命名规范  | [a-zA-Z_]\w* | 以字母或下划线开头，由字母数字下划线构成。其中通配符*表示0次或多次配前面的内容(>=0) |
URL  | ^((http&#124;https)://)?([\w-]+\.)+[\w-]+(/[\w-./?%&=]*)?$ | 通配符?表示前面的表达式出现0次或1次（0&#124;&#124;1） |
一个或多个汉字  | ^[\u0391-\uFFE5]+$ | ^在外面表示字符串开始，$表示字符串结束 |


&emsp;&emsp;最后一个特殊的\表示转义符，一些特殊符号*.?[](){}|^$\等如果需要作为字符存在则可以在他们前面加\表示该字符。但java中有点特殊，Java 源代码的字符串中的反斜线被解释为 Unicode 转义或其他字符转义。因此必须在字符串字面值中使用两个反斜线，表示正则表达式受到保护，不被 Java 字节码编译器解释，所以\d 需要\\\\d表示，\\\\需要\\\\\\\\表示。

还有些特殊用法,**贪婪与懒惰** 
  
语法 | 说明  
-|-
*? | 重复任意次，但尽可能少重复 |
+? | 重复1次或更多次，但尽可能少重复 |
?? | 重复0次或1次，但尽可能少重复 |
{n,m}? | 重复n到m次，但尽可能少重复 |
{n,}? | 重复n次以上，但尽可能少重复 |
  
  
## 二、捕获提取字符串
&emsp;&emsp;一般用于从一个字符串中解析出特定的部分，比如表达式的解析等。还有就是爬虫字符串的解析，比如图片url链接url等。Java中使用示例如下  
```java
  String line = "This order was placed for QT3000! OK?";
  String pattern = "(\\D*)(\\d+)(.*)";
 
  // 创建 Pattern 对象
  Pattern r = Pattern.compile(pattern);
 
  // 现在创建 matcher 对象
  Matcher m = r.matcher(line);
  if (m.find( )) {
      System.out.println("Found value: " + m.group(0) );
      System.out.println("Found value: " + m.group(1) );
      System.out.println("Found value: " + m.group(2) );
      System.out.println("Found value: " + m.group(3) ); 
  } else {
      System.out.println("NO MATCH");
  }
```  
其中m.group(0)代表整个表达，m.group(1)、m.group(2)、m.group(3)分别表示第一二三个括号中批判的内容
上面的运行结果如下  
```
Found value: This order was placed for QT3000! OK?
Found value: This order was placed for QT
Found value: 3000
Found value: ! OK?
```  
以上可看出小括号在正则中的特殊用法。但小括号最基本的用法是表示括号中的内容是一个整体。
捕获方法还有更高级的用法，通过group数组的方式取捕获的内容存在一定的不可靠性，java还支持给每个组取名，然后通过名称来获取捕获内容  
```java
  Pattern AGGREGATED_COLUMN_PATTERN = Pattern.compile("(?<function>\\w+)\\((?<column>[\\w#.]+)\\)");
  Matcher m = AGGREGATED_COLUMN_PATTERN.matcher(column);
  if(m.matches()) {
    return m.group("function");
  }
```  
(?<name>X)	X, as a named-capturing group，通过这种方式就可以给group组起名字了

  
## 三、替换字符串
此处的用法基本与匹配是一样的，把匹配到的字符串替换成对应的字符串
```java
  Pattern p = Pattern.compile(REGEX);
  // get a matcher object
  Matcher m = p.matcher(INPUT); 
  INPUT = m.replaceAll(REPLACE);
  //也可以直接用String的replaceAll方法
  String INPUT = "abc,123,efg";
  INPUT.replaceAll("\\d","");
```  
看源码你会发现其实两种方式是完全一样的，只是String的方法更方便。replaceFirst方法也是一样的。

## 四、分割字符串
分割字符串的时候也可以用正则表达式，这样能实现一些很强大的功能。可以看下Pattern类split方法的描述
Splits the given input sequence around matches of this pattern
```java
String s = "123\\r\\n456 789";
String[] arr = s.split("\\s");
```
String类中的split方法也是通过Pattern的split方法实现的

## 五、更多用法
Pattern更详细全面的用法可以参考[java文档](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html)  

  
*参考文档*
1. <https://www.runoob.com/java/java-regular-expressions.html>
2. <https://blog.csdn.net/mynamepg/article/details/83110538>
3. <https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html>
