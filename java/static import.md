# 什么是静态引用
只引用静态方法和静态成员的引用，可以使用通配符，引用类里的所有静态方法和静态字段，也可以只引用具体的某一个。
```java
import static com.abc.test.ClassA.*;
import static com.abc.test.ClassA.methodA;
import static com.abc.test.ClassA.FIELD_A;
```

# 什么时候用
如果一个类里会多次使用一个其他类的静态字段或方法时使用，这样在使用时就很方便。  
但这也会降低可能性，有时候类名会让代码更清晰，比如下面的MAX_VALUE表示最大值，这就有点困惑了，如果加上Integer就很清楚时int型的最大值。
```java
import static java.lang.System.out; 
import static java.lang.Integer.*; 
 
public class TestStaticImport { 
    public static void main(String[] args) { 
        out.println(MAX_VALUE); 
        out.println(MAX_VALUE + MIN_VALUE); 
    } 
}
```

# 注意事项
如果引用的两个类包含相同的静态成员变量时，直接使用成员变量名称会引起二义性，这时需要加上类名来区分。
