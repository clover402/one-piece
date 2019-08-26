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
