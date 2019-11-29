## Chap8 Collections
### 8.2 Iterator
集合删除元素时一定要**使用迭代器的remove方法**，否则会抛异常

### 8.3 Set
集合，元素不可重复，通过元素的equals方法来判断，所以equals方法一定要写好  
如果元素是可变对象则可能会在元素改变后导致异常
#### HashSet LinkedHashSet 无序
#### TreeSet 实现了SortedSet接口带排序功能
#### EnumSet 元素都是枚举

### 8.4 List
有序集合
#### ListIterator 
增加了向前迭代 和 添加元素的方法  
#### ArrayList 
相当于数组的加强版,如果需要添加大量元素可以设置initalCapacity来提高性能，默认长度10。非线程安全
#### Vector 
线程安全，老版的List，性能要比ArrayList低
##### Stack 
栈，模拟栈的数据结构，后进先出，性能不好
#### LinkedList 
基于链表的List。插入删除速度快,随机访问性能差，采用迭代器来遍历元素更好
#### Arrays.asList 
固定长度List

### 8.5 Queue
模拟队列结构，先进先出
#### PriorityQueue 
按元素大小排序，不允许传入null，可以自然排序或定制排序
#### Deque
子接口，双端队列
##### ArrayDeque
基于数组的实现类，可以当做栈来使用，性能比Stack好
##### LinkedList
Deque的另外一个实现类，可以作为双端队列或者栈来使用

### 8.6 Map
类似于php中的关联数组，python的字典  
Set的实现就是基于value全为null的Map
#### HashMap
不能保证顺序。如果添加自定义类作为key，重写equals和hashCode是要保证两个方法判断标准一致
#### LinkedHashMap 
双向链表，维护元素插入顺序
#### Hashtable 
古老的数据结构，新版本不建议使用，线程安全，不运行null作为key或者value
##### Properties Hashtable的子类，可以读取属性配置文件到Map对象
#### TreeMap 
实现了SortedMap接口，根据key对节点进行排序，有自然排序和定制排序
#### WeakHashMap
key只保留对象的弱引用，如果key所引用的对象没有被其他强引用对象引用则可能会被垃圾回收，这些key对应的节点也可能会被自动删除
#### IdentifyHashMap
两个key严格相等时（==）才认为两个key相等
#### EnumMap
key必须是单个枚举类的枚举值，根据枚举值定义的顺序排列，不允许null作为key，创建时要指定枚举类


### 8.7 HashSet与HashMap性能说明
#### 术语
* bucket: 桶，hash表里存储元素的位置
* cpacity：容量，hash表中桶的数量
* initial capacity：初始化容量
* size：尺寸，hash表中记录的数量
* load factor：负载因子（size/capacity）
* 负载极限：最大填满程度，达到时成倍增加容量，并重新分配（rehashing），默认值0.75

### 8.8 工具类Collections
* reserve 反转
* shuffle 洗牌
* sort 排序
* swap 交换两个元素的位置
* rotate 翻转 将后x个元素移到前面或者前x个元素移到后面
* binarySearch 查找
* max/min 最大最小
* fill 填充
* frequency 返回指定元素出现次数
* indexOfSubList/lastIndexOfSubList 返回子列表在父列表中出现的位置索引
* replaceAll 替换
* emptyXxx()/singetonXxx()/unmodifiableXxx() 空集合/单元素集合/不可变集合

### 8.9 Enumeration接口
Iterator迭代器的古老版本  
* hasMoreElements
* nextElements  
新集合再不支持此接口  

## Chap9 泛型
泛型是为了集合而生  
如果要继续一个包含泛型的父类，需要指明形参的具体类型  
泛型不管实际类型参数是什么，运行时总是有同样的class  
静态方法、静态变量、静态初始化块不允许使用类型参数  ？？？
  
泛型与数组有所不同：Integer[] 是 Number[]的子类，List<Integer> 并不是List<Number>的子类  
通配符:  
* ? 表示任意类型
* ? extends ClassA 表示都是ClassA及其子类
* ? super ClassA 表示都是ClassA及其父类  
非泛型转泛型则类型信息会丢失，泛型转非泛型可以编译通过但是会发出“未经检查的转换”警告  
  

## Chap10 异常处理
### Error
一般为虚拟机相关问题，如系统崩溃、虚拟机错误、动态链接失败等  
程序无需处理
### Exception
* getMessage 异常描述
* printStackTrace 将异常的跟踪栈输出
* getStackTrace 返回异常的跟踪栈信息  
finally主要进行资源的回收



