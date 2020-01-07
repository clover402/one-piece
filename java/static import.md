# 什么是静态引用
只引用静态方法和静态成员的引用，可以使用通配符，引用类里的所有静态方法和静态字段，也可以只引用具体的某一个。
```java
import static com.abc.test.ClassA.*;
import static com.abc.test.ClassA.methodA;
import static com.abc.test.ClassA.FIELD_A;
```

# 什么时候用
如果一个类里会多次使用一个其他类的静态字段或方法时使用，这样在使用时就很方便
```java
import static java.lang.System.out; 
import static java.lang.Integer.*; 
 
public class TestStaticImport { 
    public static void main(String[] args) { 
        out.println(MAX_VALUE); 
        out.println(toHexString(42)); 
    } 
}
```

