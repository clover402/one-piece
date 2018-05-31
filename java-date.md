## **Date类**
* 构造函数 ：Date()当前时间，Date(long millisec)时间戳毫秒数设置的时间
* 比较函数 ：after， before， compareTo， equals
* 时间戳转换函数：getTime 返回时间戳的毫秒，setTime 按毫秒级的时间戳设置
* 转换时间字符串：toString(), toLocaleString()

## **SimpleDateFormat类**
* 构造函数：SimpleDateFormat(String format) 格式字符串
    - y 四位年份
    - M 月份
    - d 日期
    - H 小时（24小时制）
    - h 小时（12小时制）
    - m 分钟
    - s 秒
    - E 星期几(如：Tuesday)
* format函数用法：ft.format(dateObj),参数为Date对象转换为指定格式字符串
* parse函数用法：ft.parse("2018-05-03"),指定格式字符串转换为日期对象

## **String.format方法**
用法：format("%tD", new Date()), %t开头并且以下面表格中的一个字母结尾
    * F 2018-05-31
    * D 05/31/2018
    * T 14:28:16 （24小时制）
    * R 14:28  (时分，24小时制)
    * c 星期六 十月 27 14:21:20 CST 2007
时间字段复用 String.format("%1$tF %1$tT", new Date());
String.format("%1$tY年%1$tM月%1$td日", new Date());

## sleep
Thread.sleep(long millisec); 休眠xx毫秒

## 测量时间
long start = System.currentTimeMillis( );
Thread.sleep(1000);
long end = System.currentTimeMillis( );
long diff = end - start;


