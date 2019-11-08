## 哈希表
哈希表的本质是一个数组，数组中每一个元素称为一个箱子(bin)，箱子中存放的是键值对。  
哈希表的存储过程如下:
1. 根据 key 计算出它的哈希值 h。
2. 假设箱子的个数为 n，那么这个键值对应该放在第 (h % n) 个箱子中。
3. 如果该箱子中已经有了键值对，就使用开放寻址法或者拉链法解决冲突。  
  
在使用拉链法解决哈希冲突时，每个箱子其实是一个链表，属于同一个箱子的所有键值对都会排列在链表中。

## HashMap

源码注释：不保证顺序，非线程同步的。会增加容量，不要把初始容量设置的太高或者加载因子设置的过低。有两个参数会影响Map的性能：
1. 初始容量（initial capacity）：创建hash表时的箱子大小，默认值是16，值必须是2的n（n>=4）次幂,最大值1<<30
2. 加载因子（load factor）：加载因子是表示Hsah表中元素的填满的程度.默认值0.75是一个很好地值，保证了时间和空间的平衡。当使用空间> 当前总容量*加载因子时，哈希表会自动扩容，一般会扩容为原来的两倍，并进行rehash（重哈希） 
  
如果键值很多，使用一个合适的大容量，可以防止自动rehash的发生，可以大大提高效率。  
使用大量的重复key会严重降低性能。  
如果在迭代器生成后，使用非迭代器的remove方法来修改Map都会抛异常

### 实现描述
#### 箱型哈希表（binned(bucketd) has table）
一般默认都是哈希表
#### 树形（Tree bins）
元素量特别大时默认转为树形结构。主要通过hashCode来排序。  
在key的hashCode都不同或者是可排序时, **添加元素**的时间复杂度是 O（log n），但是如果hashCode方法区分度很低性能会急剧下降  
但是TreeNode的空间是正常节点的2倍,所以只有当哈希表的箱子中链表长度大于一定的值（TREEIFY_THRESHOLD）时才会采用树形.  
如果元素数量变少，会转化回原来的箱型。
hashCodes区分度很高冲突很少时，树形结构很少用到

### 属性
#### transient Node<K,V>[] table
实际存储数据的结构，是一个链表数组，每个元素Node都是一个联表。树形结构的TreeNode实际上也是它的子类。不参与序列化
#### transient Set<Map.Entry<K,V>> entrySet
所有元素节点，真正存数据的结构

###  public方法
1. boolean containsKey(Object key)
2. boolean containsValue(Object value)
3. V put(K key, V value)
4. V putIfAbsent(K key, V value):不存在才放入
5. void putAll(Map<? extends K, ? extends V> m)
5. V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction):如果key对应的值为空，则生成一个并存进去
5. V computeIfPresent(K key, Function<? super K, ? extends V> mappingFunction): 与上面的相反
5. V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction):key对应的不存在用value，存在把两个value传入通过函数计算得到最终结果存入
6. V remove(Object key)
7. void clear()
8. V get(Object key)
8. V getOrDefault(Object key, V defaultValue)：获取不到就取默认值
9. boolean replace(K key, V oldValue, V newValue)
9. V replace(K key, V value)
9. void replaceAll(BiFunction<? super K, ? super V, ? extends V> function)：用给定函数的计算结果替换所有的value 
10. Set<K> keySet()
11. Collection<V> values()
12. Set<Map.Entry<K,V>> entrySet()  
13. void forEach(BiConsumer<? super K, ? super V> action) 

## ConcurrentHashMap
