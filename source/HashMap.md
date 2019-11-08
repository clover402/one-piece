## HashMap
源码注释：不保证顺序，非线程同步的。会增加容量，不要把初始容量设置的太高或者加载因子设置的过低。有两个参数会影响Map的性能：
1. 初始容量（initial capacity）：创建hash表时的空间大小，默认值是16，值必须是2的n（n>=4）次幂,最大值1<<30
2. 加载因子（load factor）：加载因子是表示Hsah表中元素的填满的程度.默认值0.75是一个很好地值，保证了时间和空间的平衡  
  
如果键值很多，使用一个合适的大容量，可以防止自动rehash的发生，可以大大提高效率。  
使用大量的重复key会严重降低性能。  
如果在迭代器生成后，使用非迭代器的remove方法来修改Map都会抛异常

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
