## php小知识
1. do-while(0)意义。
* 避免goto语句，如果后面有部分语句在特定情况下不需要执行，只要把他们放到do-while(0)里面，在合适的地方break即可
* 用作mini函数，把整个语句块当做一个函数，可以定义局部变量
* c语言中定义宏
2. list函数。
* 该函数只用于数字索引的数组，且假定数字索引从 0 开始,依次获取。list中的单元可以少于嵌套数组的，此时多出来的数组单元将被忽略。
* list可以给返回数组的函数解包，这样函数可以返回多个值
* list可以把嵌套的数组解包到循环变量
```
<?php
$array = [
    [1, 2],
    [3, 4],
];

foreach ($array as list($a, $b)) {
    // $a contains the first element of the nested array,
    // and $b contains the second element.
    echo "A: $a; B: $b\n";
}
```
3. 字母[a-zA-Z]可以自增，效果类似26进制数据，如 'Z'++变为'AA',但字母不能自减
4. 执行运算符可以用``引用系统命令来执行
5. continue 接受一个可选的数字参数来决定跳过几重循环到循环结尾。默认值是1，即跳到当前循环末尾。
6. declare(ticks=N){ statement; }或 declare(ticks=N);statement;
* PHP编译会在statement(语句), function_declaration_statement（函数定义语句）， class_declaration_statement后插入TICKS处理函数，即它会在每条statement，函数声明，类（实际上还包括接口）声明后插入一条TICKS指令。一个函数的定义包括完整的函数原形及函数体算一条，完整的class或interface定义算是一个class_declration_statement。statement包括：
(1) 简单语句：空语句（就一个；号），return,break,continue,throw, goto,global,static,unset,echo, 内置的HTML文本，分号结束的表达式等均算一个语句。
(2) 复合语句：完整的if/elseif,while,do...while,for,foreach,switch,try...catch等算一个语句。
(3) 语句块：{} 括出来的语句块。
(4) 最后特别的：declare块本身也算一个语句(按道理declare块也算是复合语句，但此处特意将其独立出来)。

* declare 结构也可用于全局范围，影响到其后的所有代码（但如果有 declare 结构的文件被其它文件包含，则对包含它的父文件不起作用）。可用tick来进行调试，性能测试，实现简单的多任务，或者做一些后台的I/O操作等等。
* Zend引擎每执行N条低级语句就去执行一次 register_tick_function() 注册的函数。
可以粗略的理解为每执行N句php代码（例如:$num=1;）就去执行下已经注册的tick函数。
一个用途就是控制某段代码执行时间，例如下面的代码虽然最后有个死循环，但是执行时间不会超过5秒。
```
<?php
declare(ticks=1);

// 开始时间
$time_start = time();

// 检查是否已经超时
function check_timeout(){
    // 开始时间
    global $time_start;
    // 5秒超时
    $timeout = 5;
    if(time()-$time_start > $timeout){
        exit("超时{$timeout}秒\n");
    }
}

// Zend引擎每执行一次低级语句就执行一下check_timeout
register_tick_function('check_timeout');

// 模拟一段耗时的业务逻辑
while(1){
   $num = 1;
}

// 模拟一段耗时的业务逻辑，虽然是死循环，但是执行时间不会超过$timeout=5秒
while(1){
   $num = 1;
}
```
* declare(ticks=1);每执行一次低级语句会检查一次该进程是否有未处理过的信号,测试代码如下：
运行 php signal.php
然后CTL+c 或者 kill -SIGINT PID 会导致运行代码跳出死循环去运行pcntl_signal注册的函数，效果就是脚本exit打印“Get signal SIGINT and exi”退出
```
<?php
declare(ticks=1);
pcntl_signal(SIGINT, function(){
   exit("Get signal SIGINT and exit\n");
});

echo "Ctl + c or run cmd : kill -SIGINT " . posix_getpid(). "\n" ;

while(1){
  $num = 1;
}
```
* 性能测试
```
function profile() {
    global $tmp;
    printf("Now tmp is %d.n<br/>",$tmp);
}
//注册tick方法
register_tick_function("profile");
//设定每执行几条语句执行已注册的方法这里设置了3条/每次
declare(ticks=3) {//受tick监控的代码段
    $tmp = 1;//一条简单语句
    $tmp = 2;//一条简单语句
    $tmp = 3;//一条简单语句
    $tmp = 4;//一条简单语句
    $tmp = 5;//一条简单语句
    $tmp = 6;//一条简单语句
    $tmp = 7;//一条简单语句
    $tmp = 8;//一条简单语句
    //unregister_tick_function("profile");
}
```
