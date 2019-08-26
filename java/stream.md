# stream

## 什么是stream（流）
Stream 不是集合元素，它不是数据结构并不保存数据，它是有关算法和计算的，它更像一个高级版本的 Iterator。
单向，不可往复，数据只能遍历一次，遍历过一次后即用尽了，就好比流水从面前流过，一去不复返。
Stream 可以并行化操作，使用并行去遍历时，数据会被分成多个段，其中每一个都在不同的线程中处理，然后将结果一起输出。
Stream 的另外一大特点是，数据源本身可以是无限的。

## stream的构成
通常包括三个基本步骤：
获取一个数据源 --> 数据转换 --> 执行操作获取想要的结果
![流管道 (Stream Pipeline) 的构成](https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/img001.png)  
这有点像linux命令中的管道操作符|
### 数据源
* Collection: Collection.stream, Collection.parallelStream()
* 数组: Arrays.stream(T array)
* BufferedReader: java.io.BufferedReader.lines()
* Spliterator: java.util.Spliterator
* 其他
### Intermedia
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

### Intermedia
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
说明：


