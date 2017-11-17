## php高级技巧
### php小知识
1. goto可代替多层的break
3. 字母[a-zA-Z]可以自增，效果类似26进制数据，如 'Z'++变为'AA',但字母不能自减
4. 执行运算符可以用``引用系统命令来执行
5. continue 接受一个可选的数字参数来决定跳过几重循环到循环结尾。默认值是1，即跳到当前循环末尾。
6. set_error_handler()可以自定义错误处理函数，php7多数错误以Error类抛出，可以用 catch (Error $e) { ... }，或者通过注册异常处理函数（ set_exception_handler()）来捕获 Error。
### do-while(0)意义。
* 避免goto语句，如果后面有部分语句在特定情况下不需要执行，只要把他们放到do-while(0)里面，在合适的地方break即可
* 用作mini函数，把整个语句块当做一个函数，可以定义局部变量
* c语言中定义宏
### list函数
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

### declare(ticks=N)
* 语法：declare(ticks=N){ statement; }或 declare(ticks=N);statement;
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

### 类与对象
1. 同一个类的对象即使不是同一个实例也可以互相访问对方的私有与受保护成员。这是由于在这些对象的内部具体实现的细节都是已知的。
```
<?php
class Test
{
    private $foo;

    public function __construct($foo)
    {
        $this->foo = $foo;
    }

    private function bar()
    {
        echo 'Accessed the private method.';
    }

    public function baz(Test $other)
    {
        // We can change the private property:
        $other->foo = 'hello';
        var_dump($other->foo);

        // We can also call the private method:
        $other->bar();
    }
}

$test = new Test('test');
$test->baz(new Test('other'));
```
2. 抽象类：方法的调用方式必须匹配，即类型和所需参数数量必须一致。例如，子类定义了一个可选参数，而父类抽象方法的声明里没有，则两者的声明并无冲突。 这也适用于 PHP 5.4 起的构造函数。在 PHP 5.4 之前的构造函数声明可以不一样的。
3. Trait：
* 从基类继承的成员会被 trait 插入的成员所覆盖。优先顺序是来自当前类的成员覆盖了 trait 的方法，而 trait 则覆盖了被继承的方法。
* 通过逗号分隔，在 use 声明列出多个 trait，可以都插入到一个类中。
* 如果两个 trait 都插入了一个同名的方法，如果没有明确解决冲突将会产生一个致命错误。
为了解决多个 trait 在同一个类中的命名冲突，需要使用 insteadof 操作符来明确指定使用冲突方法中的哪一个。
* 以上方式仅允许排除掉其它方法，as 操作符可以 为某个方法引入别名。 注意，as 操作符不会对方法进行重命名，也不会影响其方法。
* 使用 as 语法还可以用来调整方法的访问控制。
```
<?php
trait A {
    public function smallTalk() {
        echo 'a';
    }
    public function bigTalk() {
        echo 'A';
    }
}

trait B {
    public function smallTalk() {
        echo 'b';
    }
    public function bigTalk() {
        echo 'B';
    }
}

class Talker {
    use A, B {
        B::smallTalk insteadof A;
        A::bigTalk insteadof B;
    }
}

class Aliased_Talker {
    use A, B {
        B::smallTalk insteadof A;
        A::bigTalk insteadof B;
        B::bigTalk as talk;
    }
}
```
4. **魔术方法：**
* __sleep() 主要用于serialize，serialize() 函数会检查类中是否存在一个魔术方法 __sleep()。如果存在，该方法会先被调用，然后才执行序列化操作。此功能可以用于清理对象，并返回一个包含对象中所有应被序列化的变量名称的数组。如果该方法未返回任何内容，则 NULL 被序列化，并产生一个 E_NOTICE 级别的错误。__sleep() 不能返回父类的私有成员的名字，常用于提交未提交的数据，或类似的清理操作。同时，如果有一些很大的对象，但不需要全部保存，这个功能就很好用。
* __wakeup() 则相反，用于unserialize
* __toString() 方法用于一个类被当成字符串时应怎样回应。例如 echo $obj; 应该显示些什么。此方法必须返回一个字符串，否则将发出一条 E_RECOVERABLE_ERROR 级别的致命错误。不能在 __toString() 方法中抛出异常。这么做会导致致命错误。
* __invoke() 当尝试以调用函数的方式调用一个对象时，__invoke() 方法会被自动调用。
* __set_state() 自 PHP 5.1.0 起当调用 var_export() 导出类时，此静态 方法会被调用。本方法的唯一参数是一个数组，其中包含按 array('property' => value, ...) 格式排列的类属性。
* __debugInfo() var_dump()对一个对象调用时会触发此方法
* __call() 在对象中调用一个不可访问方法时触发
* __callStatic() 在静态上下文中调用一个不可访问方法时触发
* __set(),__get(),__isset(),__unset() 属性重载
* __clone() 对象复制可以通过 clone 关键字来完成（如果可能，这将调用对象的 __clone() 方法）。对象中的 __clone() 方法不能被直接调用.当对象被复制后，PHP 5 会对对象的所有属性执行一个浅复制（shallow copy）。所有的引用属性仍然会是一个指向原来的变量的引用。当复制完成时，如果定义了 __clone() 方法，则新创建的对象（复制生成的对象）中的 __clone() 方法会被调用，可用于修改属性的值（如果有必要的话）。
5. 后期静态绑定：该功能从语言内部角度考虑被命名为"后期静态绑定"。"后期绑定"的意思是说，static:: 不再被解析为定义当前方法所在的类，而是在实际运行时计算的。也可以称之为"静态绑定"，因为它可以用于（但不限于）静态方法的调用。后期静态绑定的解析会一直到取得一个完全解析了的静态调用为止。另一方面，如果静态调用使用 parent:: 或者 self:: 将转发调用信息。


### 生成器generator
1. 核心是yeild命令，与函数、foreach结合使用
2. 主要用于解决大数组问题，减少内存占用
3. yeild可以与from一起使用来从数组、函数或其他生成器中获取数据
```
<?php
function gen_one_to_three() {
    for ($i = 1; $i <= 3; $i++) {
        //注意变量$i的值在不同的yield之间是保持传递的。
        yield $i;
    }
}

$generator = gen_one_to_three();
foreach ($generator as $value) {
    echo "$value\n";
}

//生成带键值的迭代器
function input_parser($input) {
    foreach (explode("\n", $input) as $line) {
        $fields = explode(';', $line);
        $id = array_shift($fields);

        yield $id => $fields;
    }
}


```




