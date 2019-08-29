# JAVA编程思想（第四版）
## 类相关  
1. 原始型数据类型如果是类的成员变量会给默认的初始值，但如果是函数里的局部变量则不会给默认值.对象在类内会默认初始化为null
2. 设计构造函数时一个特别有效的规则是：用尽可能简单的方法使对象进入就绪状态；如果可能，避免调用任何方法。在构建器内唯一能够安全调用的是在基础类中具有final 属性的那些方法（也适用于private方法，它们自动具有final 属性）。  
如果构造函数调用了其他方法，而这个方法在子类被覆盖可能会导致一个为初始化的变量被使用，从而出现异常  
3. 设计类时合成优先于继承  

## 集合  
1. Enumeration：类似于迭代器，遍历集合用的
2. 集合类型：
* Vector：基础的集合类，线性集合，与数组类似，只是可以动态扩张
* BitSet：由二进制位构成的集合，为了保存大量“开-关”信息，最小长度64位  
* Stack：栈，后进先出
* Hashtable：哈希表，通过键值指定元素
3. 新集合：
* Collection: 集合，一组应用了某种规则的单独元素。List按特定顺序容纳元素，Set不包含任何重复的元素
* Map：映射，一系列键值对。Map可返回包含自己键的Set，和包含值的List，以及包含键值对的List
4. Iterator：新迭代器，取代Enumeration
5. Collections：工具类，包含方法max，min，emptyList,copy，subList，unmodifiableCollection，reverse，shuffle，swap，fill，sort，binarySearch等


