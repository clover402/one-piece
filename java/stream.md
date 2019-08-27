# stream

## 什么是stream（流）
Stream 不是集合元素，它不是数据结构并不保存数据，它是有关算法和计算的，它更像一个高级版本的 Iterator。
单向，不可往复，数据只能遍历一次，遍历过一次后即用尽了，就好比流水从面前流过，一去不复返。
Stream 可以并行化操作，使用并行去遍历时，数据会被分成多个段，其中每一个都在不同的线程中处理，然后将结果一起输出。
Stream 的另外一大特点是，数据源本身可以是无限的。

## stream的构成
通常包括三个基本步骤：
获取一个数据源 --> 数据转换（Intermediate） --> 执行操作获取想要的结果（terminal）
![流管道 (Stream Pipeline) 的构成](https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/img001.png)  
这有点像linux命令中的管道操作符|
### 数据源
* Collection: Collection.stream, Collection.parallelStream()
* 数组: Arrays.stream(T array)
* BufferedReader: java.io.BufferedReader.lines()
* Spliterator: java.util.Spliterator
* 其他
### Intermediate
一个流可以后面跟随零个或多个 intermediate 操作。其目的主要是打开流，做出某种程度的数据映射/过滤，然后返回一个新的流，交给下一个操作使用。
### Terminal
一个流只能有一个 terminal 操作，当这个操作执行后，流就被使用“光”了，无法再被操作。所以这必定是流的最后一个操作。
### short-circuiting
* 对于一个 intermediate 操作，如果它接受的是一个无限大（infinite/unbounded）的 Stream，但返回一个有限的新 Stream。如：limit
* 对于一个 terminal 操作，如果它接受的是一个无限大的 Stream，但能在有限的时间计算出结果。如：anyMatch、 allMatch、 noneMatch、 findFirst、 findAny

## stream的使用
### 构造stream的常见方法
```java
// 1. Individual values
Stream stream = Stream.of("a", "b", "c");
// 2. Arrays
String [] strArray = new String[] {"a", "b", "c"};
stream = Stream.of(strArray);
stream = Arrays.stream(strArray);
// 3. Collections
List<String> list = Arrays.asList(strArray);
stream = list.stream();
```  
需要注意的是，对于基本数值型，目前有三种对应的包装类型 Stream：  
IntStream、LongStream、DoubleStream。当然我们也可以用 Stream<Integer>、Stream<Long> >、Stream<Double>，但是 boxing 和 unboxing 会很耗时，所以特别为这三种基本数值型提供了对应的 Stream。  

### Intermediate操作

#### 1.map
作用就是把 input Stream 的每一个元素，映射成 output Stream 的另外一个元素  
**定义：<R> Stream<R> map(Function<? super T, ? extends R> mapper);**  
说明：函数或lambda入参T及其父类，返回值R及其子类
```java
//转换大写
List<String> output = wordList.stream().
map(String::toUpperCase).
collect(Collectors.toList());

//平方数
List<Integer> nums = Arrays.asList(1, 2, 3, 4);
List<Integer> squareNums = nums.stream().
map(n -> n * n).
collect(Collectors.toList());
```

#### 2.flatMap
map 生成的是个 1:1 映射，每个输入元素，都按照规则转换成为另外一个元素。还有一些场景，是一对多映射关系的，这时需要 flatMap。
flatMap 把 input Stream 中的层级结构扁平化，就是将最底层元素抽出来放到一起，最终 output 的新 Stream 里面已经没有 List 了，都是直接的数字。  
**定义：<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);**  
说明：函数或者lambda入参是T及其父类，返回值是Stream及其子类
```java
Stream<List<Integer>> inputStream = Stream.of(
 Arrays.asList(1),
 Arrays.asList(2, 3),
 Arrays.asList(4, 5, 6)
 );
Stream<Integer> outputStream = inputStream.
flatMap((childList) -> childList.stream());
```

#### 3.filter
filter 对原始 Stream 进行某项测试，通过测试的元素被留下来生成一个新 Stream。  
**定义：Stream<T> filter(Predicate<? super T> predicate);**   
说明：入参T及其父类，返回值布尔值。这是一个断言，通过一个参数，进行逻辑判断最终返回boolean
```java
 List<String> output = reader.lines().
 flatMap(line -> Stream.of(line.split(REGEXP))).
 filter(word -> word.length() > 0).
 collect(Collectors.toList()); 
```
 
#### 4.sorted
对stream进行排序，它比数组的排序更强之处在于你可以首先对 Stream 进行各类 map、filter、limit、skip 甚至 distinct 来减少元素数量后，再排序，这能帮助程序明显缩短执行时间。
有两种排序，带参数的和不带参数的。不带参数的是按照natural order进行排序，对于复杂的对象需要实现Comparable接口来使用。带参数的则在参数中定义排序规则。  
**定义：Stream<T> sorted(Comparator<? super T> comparator);**  
说明：主要是要实现这样一个方法int compare(T o1, T o2);的Comparator类。两个同类型的方法返回一个比较的数值
```java
List<Person> persons = new ArrayList();
 for (int i = 1; i <= 5; i++) {
 Person person = new Person(i, "name" + i);
 persons.add(person);
 }
List<Person> personList2 = persons.stream().limit(2).sorted((p1, p2) -> p1.getName().compareTo(p2.getName())).collect(Collectors.toList());
System.out.println(personList2);
```

#### 5.distinct
去重。定义有句描述 according to {@link Object#equals(Object)}。也就是说他是通过这个类的equals方法实现的，如果是自定义类需要重写equals方法，否则所有的对象都不会相等。


#### 6.limit/skip
limit返回前面n个元素  
skip扔掉前面n个元素

### terminal操作

#### 1.max/min/count
* long count(); 统计数量
* Optional<T> max(Comparator<? super T> comparator); 返回最大的元素，与排序一样需要传实现了比较接口的类
* Optional<T> min(Comparator<? super T> comparator); 返回最小的元素

#### 2.reduce
上面的3种都可以通过reduce来实现
* Optional<T> reduce(BinaryOperator<T> accumulator);
* T reduce(T identity, BinaryOperator<T> accumulator); 通过一个初始值和一个函数进行计算返回一个值
```java
 Integer sum = integers.reduce(0, (a, b) -> a+b);
 Integer sum = integers.reduce(0, Integer::sum);
 // 字符串连接，concat = "ABCD"
 String concat = Stream.of("A", "B", "C", "D").reduce("", String::concat); 
 // 求最小值，minValue = -3.0
 double minValue = Stream.of(-1.5, 1.0, -3.0, -2.0).reduce(Double.MAX_VALUE, Double::min); 
 // 求和，sumValue = 10, 有起始值
 int sumValue = Stream.of(1, 2, 3, 4).reduce(0, Integer::sum);
 // 求和，sumValue = 10, 无起始值
 sumValue = Stream.of(1, 2, 3, 4).reduce(Integer::sum).get();
 // 过滤，字符串连接，concat = "ace"
 concat = Stream.of("a", "B", "c", "D", "e", "F").
 filter(x -> x.compareTo("Z") > 0).
 reduce("", String::concat);
```   

#### 3.collect
* **<R> R collect(Supplier<R> supplier, BiConsumer<R,? super T> accumulator, BiConsumer<R,R> combiner)**  
说明：**supplier** - a function that creates a new result container. For a parallel execution, this function may be called multiple times and must return a fresh value each time.  
 结果存放容器  
**accumulator** - an associative, non-interfering, stateless function for incorporating an additional element into a result.  
 为结果如何添加到容器的操作  
**combiner** - an associative, non-interfering, stateless function for combining two values, which must be compatible with the accumulator function  
多个容器的聚合操作
```java
 String concat = stringStream.collect(StringBuilder::new, StringBuilder::append,StringBuilder::append).toString();
 //等价于上面,这样看起来应该更加清晰
 String concat = stringStream.collect(() -> new StringBuilder(),(l, x) -> l.append(x), (r1, r2) -> r1.append(r2)).toString();
 Lists.<Person>newArrayList().stream()
      .collect(() -> new HashMap<Integer,List<Person>>(),
          (h, x) -> {
            List<Person> value = h.getOrDefault(x.getType(), Lists.newArrayList());
            value.add(x);
            h.put(x.getType(), value);
          },
          HashMap::putAll
  );
```
* **<R,A> R collect(Collector<? super T,A,R> collector)**  
这个是上面那种的高级用法。Collector接口是使得collect操作强大的终极武器,对于绝大部分操作可以分解为旗下主要步骤,提供初始容器->加入元素到容器->并发下多容器聚合->对聚合后结果进行操作,同时Collector接口又提供了of静态方法帮助你最大化的定制自己的操作,官方也提供了Collectors这个类封装了大部分的常用收集操作.   
另外CollectorImpl为Collector的实现类,因为接口不可实例化,这里主要完成实例化操作.
```java
    //初始容器
    Supplier<A> supplier();
    //加入到容器操作
    BiConsumer<A, T> accumulator();
    //多容器聚合操作
    BinaryOperator<A> combiner();
    //聚合后的结果操作
    Function<A, R> finisher();
    //操作中便于优化的状态字段
    Set<Characteristics> characteristics();
```
##### Collectors
* toList()/toSet()/toCollection()  
创建容器: ArrayList::new  
加入容器操作: List::add  
多容器合并: left.addAll(right); return left;  
聚合后的结果操作: 这里直接返回,因此无该操作,默认为castingIdentity()  
优化操作状态字段: CH_ID  
这样看起来很简单,那么对于toCollection,toSet等操作都是类似的实现.
```java
public static <T>
    Collector<T, ?, List<T>> toList() {
        return new CollectorImpl<>((Supplier<List<T>>) ArrayList::new, List::add,
                                   (left, right) -> { left.addAll(right); return left; },
                                   CH_ID);
    }
```

* toMap  
```java
Collector<T, ?, Map<K,U>> toMap(Function<? super T, ? extends K> keyMapper,  Function<? super T, ? extends U> valueMapper)
Collector<T, ?, Map<K,U>> toMap(Function<? super T, ? extends K> keyMapper,Function<? super T, ? extends U> valueMapper,
                                    BinaryOperator<U> mergeFunction)
Collector<T, ?, M> toMap(Function<? super T, ? extends K> keyMapper, Function<? super T, ? extends U> valueMapper,
                                BinaryOperator<U> mergeFunction, Supplier<M> mapSupplier)                                    
```
实际上只4个参数，另外两种是有默认值的。  
参数1：keyMapper， 生成键的函数引用或者lambda  
参数2：valueMapper，生成值的函数引用或者lambda  
参数3：mergeFunction，a merge function, used to resolve collisions between values associated with the same key.对于重复key的处理函数  
参数4：mapSupplier，a function which returns a new, empty into which the results will be inserted.放最终结果的容器  

* joining  
合并连接字符串，可加分隔符、前缀、后缀

* groupingBy  
groupingBy是toMap的一种高级方式,弥补了toMap对值无法提供多元化的收集操作,比如对于返回Map<T,List<E>>这样的形式toMap就不是那么顺手,那么groupingBy的重点就是对Key和Value值的处理封装  
```java
 <T, K, D, A, M extends Map<K, D>>
 Collector<T, ?, M> groupingBy(Function<? super T, ? extends K> classifier, Supplier<M> mapFactory, Collector<? super T, A, D>  downstream)
``` 



#### 4.forEach

#### 5.findFirst

#### 6.findAny

#### 7.allMatch/anyMatch/noneMatch
* allMatch：Stream 中全部元素符合传入的 predicate，返回 true



