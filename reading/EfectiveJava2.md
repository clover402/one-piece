## 方法设计规范
1. 检查参数有效性：文档中必须说明限制，方法体开始对参数进行检查
2. 如果要保证类的成员变量不能被外部随意改动，可以对引用参数进行保护性拷贝，但不要用clone方法
3. 方法功能要完整，只有频繁操作才提供快捷方法
4. 避免长长的参数列表，一般不应超过3个参数
5. 参数类型接口优于类
6. 谨慎使用重载，到底调用哪个重载是在编译时就确定的
7. 返回零长度的数组而不是null
8. 方法需要注释文档，文档应该说明方法做了什么，而不是如何做的
9. 文档注释还需要描述前提条件（未被检查的异常、受影响的参数）
和后置条件以及副作用（如：方法启动了一个后台线程）
10. 方法的概要描述是一个动词短语，类的概要描述是一个名词短语

## 其他注意事项
1. 变量作用域最小化
2. 熟悉库函数能用库函数尽量库函数，减少重复造轮子
3. 货币计算请使用BigDecimal而不是double和float
4. 经常变化的字符串请使用StringBuffer而不是String
5. 优化容易带来伤害，一定要确保优化是成熟的深思熟虑的。在设计过程中考虑性能问题
6. 异常分3种：需要检查的异常、未被检查的异常（RumtimeException子类）、错误
7. 保持异常失败原子性，失败时不会影响对象的状态（类似db的事务回滚）。任何一个异常都不应该改变对象调用该方法之前的状态
8. 不要忽略异常
9. 同步使用syschronized和violatile。同步区域内要控制行为，避免可能引起死锁的操作，也可以把耗时长的操作移到外面可以增加并发。减少不不要的同步，同步会带来性能问题。所有的wait都应该在循环内部。

## 类相关
1. 如果类可以设计为非可变类尽量设计为非可变类
2. 复合优于继承，只有确实是is-a关系时才使用继承
3. 设计可被继承的类时，构造函数不要调用其他函数
4. 接口优于抽象类。骨架实现类可以作为接口的实现。设计公有接口要十分谨慎，
因为一旦被公开且使用，再想修改是不可能的。
5. 不用使用常量接口，用工具类来代替
6. 如果成员类不需要调用外围类则要把它申明为静态的，这样就不会生产对外围类的引用
7. 使用接口和实现接口的类来代替函数指针
8. 用接口做变量类型优于类，抽象基类优于子类

## 线程相关
1. 对大多数程序员来说，Thread.yield的唯一用途是测试期间人为的增加一个程序的并发性
