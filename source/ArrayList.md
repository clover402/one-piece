# ArrayList
## 属性
### DEFAULT_CAPACITY = 10
默认容量大小
### Object[]
实际存储数据用对象数组 
### size
ArrayList长度 
### MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8
ArrayList可用最大长度

## private方法
### grow
不传参则增长为size+1的容量。
### newCapacity
获取最新容量大小，newCapacity为元素对象数组大小增长50%的大小，minCapacity为希望的新容量大小。  
如果newCapacity小于等于minCapacity，则以minCapacity为准，当然如果元素对象数组为空，则表示还没有添加过元素，那么返回默认的容量大小10.  
如果newCapacity大于minCapacity，则以newCapacity为准，这样可以减少扩容次数。如果容量超过了最大可用长度，则使用Integer.MAX_VALUE,不能再大了。
### hugeCapacity
获取超大容量，传参大于MAX_ARRAY_SIZE就用Integer的最大值，否则使用MAX_ARRAY_SIZE

### fastRemove
remove的简化版，不做边界检查，不返回被删除元素

### removeRange(int fromIndex, int toIndex)
把toIndex之后的所有元素移动到fromIndex位置，把新size之后的所有元素设置为null

### batchRemove(Collection c, boolean complement)
移除方法调用时complement是false，保留方法调用时为true，结果就是移除时把不在c的一点点挪到数组最前面，保留时把在c的移到数组最前面，同时会获得剩余元素个数w，此处的for循环很有技巧，不用产生新空间。  
当因为某些特殊原因导致没有遍历完元素数组时，把r后面的所有元素挪到w后面，并更新w大小为新值。  
如果w小于size，则说明一定有有元素被干掉了，遍历数组把干掉的元素都设置为null，更新size为w。


## public方法
### trimToSize 
让实际容量（对象数组大小）与size保持一致

### ensureCapacity 
确保容量有够大小。数据数组为空，且要确保的容量小于10，不执行任何操作。  

### indexOf
获取某元素对象首次在数组出现的位置，不存在返回-1.

### lastIndexOf
获取某元素对象最后一次在数组出现的位置，不存在返回-1.

### clone
浅拷贝，元素对象只复制引用。

### toArray
浅拷贝，元素对象只是复制引用

### get
判断获取得位置是否合法，合法则返回对应位置得元素

### set
判断位置是否合法，合法则取出原值，设置新值，返回原值

### add(E e)
如果size与数据元素数组大小一样，则让数据元素数组大小扩容。第一次添加元素时，size为0，元素数组大小也为0，所以也会发生扩容，此时会扩容为默认容量大小10.

### add(int index, E element)
如果为初始空数组状态，创建默认容量大小10的数组，否则确保数组容量大小至少比当前大小大1.将index之后的所有元素向后移动一格，不创建新对象数组，再原数组上操作。将数组index位置设置为element

### allAll(Collection<? Extends E> c)
确保空间满足size+c.size,从size位置开始，将c数组内容复制过去，size增加对应长度

### remove(int index)
将要删除的元素位置之后的元素整体向前移动一格，将最后一个位置的元素设置为null

### remove(Object o)
如果o为null，则遍历数组，找到第一个等于null的删除。如果不为null，则遍历数组找到一个equals的对象删除

### removeAll(Collection<?> c)
移除集合内所有元素，调用batchRemove方法实现

### retainAll(Collection<?> c)
维持集合内所有元素，也是调用batchRemove方法

### clear
所有元素设置为null，size设置为0


### listItrator(int index)
返回一个从index开始的list迭代器，不传参则是从0开始。ListItr功能更强可以向前移动

### iterator
返回一个普通迭代器Itr

### subList(int fromIndex, int toIndex)
返回一个SubList对象，还是基于原数据，只是增加了offset，size等属性，和相关的核心方法

### forEach(Consumer<? super E> action)
遍历数组，对它执行无返回值lambda

### spliterator
返回一个ArrayListSpliterator对象，该类有对半分割函数，遍历所有对象或者剩余对象执行Consumer函数等方法。主要用于将大的list分割后用于多线程。

### removeIf(Predicate<? super E> filter)
用断言lambda删除元素.这里使用了一个BitSet来标记需要删除的元素的index。如果要删除数量大于0，遍历数组把BitSet中所有未标记的元素依次移动到数组最左边，后续的元素全部设置为null。

### replaceAll(UnaryOperator<E> operator)
遍历元素，对所有元素执行operator函数

### sort(Comparator<? super E> c)
通过排序函数（实现了compare(E o1, E o2)方法的类）对数组元素进行排序，调用Arrays.sort
