# 1. Intruduction
>This document serves as the complete definition of Google's coding standards for source code in the Java™ Programming Language. 
A Java source file is described as being in Google Style if and only if it adheres to the rules herein.

此文档作为谷歌编码规范的完整定义，用于JAVA语言编写的源代码。当且仅当一个JAVA源文件遵从此处的规则才能被称为谷歌风格。

>Like other programming style guides, the issues covered span not only aesthetic issues of formatting, but other types of conventions 
or coding standards as well. However, this document focuses primarily on the hard-and-fast rules that we follow universally, 
and avoids giving advice that isn't clearly enforceable (whether by human or tool).  

像其他的编程风格指导，议题包含的范围不仅仅是格式化的美观问题，还包含其他类型的惯例和编码标准。无论如何，此文档主要关注于我们普遍遵循的硬性规则，避免给出
一些不能被明确执行（无论是人还是工具）的建议

## 1.1 Terminology notes(术语说明)
>In this document, unless otherwise clarified:  
1.The term class is used inclusively to mean an "ordinary" class, enum class, interface or annotation type (@interface).  
2.The term member (of a class) is used inclusively to mean a nested class, field, method, or constructor; 
that is, all top-level contents of a class except initializers and comments.  
3.The term comment always refers to implementation comments. We do not use the phrase "documentation comments", 
instead using the common term "Javadoc."  
Other "terminology notes" will appear occasionally throughout the document.  

此文档中，除非除了明确说明的：
1. 类class用于表示一个普通的类、枚举类、接口或者注解类型（@interface）。
2. (类的)成员用于表示一个内嵌类、成员变量、成员方法或者构造函数；也就是，一个类除了初始化块和注释的所有顶级内容。
3. 注释一直都是指的执行注释，我们不使用短语“文档注释”，用Javadoc代替  
  
其他术语说明再后面的整个文档中都可能会出现

## 1.2 Guide notes
>Example code in this document is non-normative. That is, while the examples are in Google Style, they may not illustrate 
the only stylish way to represent the code. Optional formatting choices made in examples should not be enforced as rules.

此文档中的中的示例代码不是正式的。也就是说，那些使用谷歌风格的示例，并不表示他们是实现代码的唯一优雅方式。示例中使用可选的格式化选择不应该被强加为规则。

# 2. Source file basics
## 2.1 File Name
>The source file name consists of the case-sensitive name of the top-level class it contains (of which there is exactly one), 
plus the .java extension.

源文件的名字由它包含的（只有一个）大小写敏感的顶级类名加上.java扩展名构成。

## 2.2 File encode: UTF-8
>Source files are encoded in UTF-8. 源文件全部用utf-8编码

## 2.3 Special characters
### 2.3.1 Whitespace characters
>Aside from the line terminator sequence, the ASCII horizontal space character (0x20) is the only whitespace character that appears anywhere in a source file. This implies that:  
1.All other whitespace characters in string and character literals are escaped.  
2.Tab characters are not used for indentation.

除了行终结符之外，ASCII码水平空白字符(0x20)是唯一一个可以在源文件中可以任意出现的空白字符。这表明：
1. 所有的其他string和character形式的空白字符需要转义
2. Tab字符不能用于缩进  

### 2.3.2 Special escape sequence
>For any character that has a special escape sequence (\b, \t, \n, \f, \r, \", \' and \\), that sequence is used rather than the corresponding octal (e.g. \012) or Unicode (e.g. \u000a) escape.  

对于任意使用了转义的字符 (\b, \t, \n, \f, \r, \", \' 和 \\)，要使用转义符而不是对应的八进制和unicode转义

### 2.3.3 Non-ASCII characters
>For the remaining non-ASCII characters, either the actual Unicode character (e.g. ∞) or the equivalent Unicode escape (e.g. \u221e) is used. The choice depends only on which makes the code easier to read and understand, although Unicode escapes outside string literals and comments are strongly discouraged.  
Tip: In the Unicode escape case, and occasionally even when actual Unicode characters are used, an explanatory comment can be very helpful.

对于余下的非ASCII字符，要么直接使用unicode字符（比如 ∞），要使用等价的转义Unicode码（比如 \u221e)。至于选择哪一个仅仅依赖于哪种方式使代码更容易阅读和理解，然而对于非string语法和注释的转义unicode码强烈不推荐。  
提示：使用转义unicode的情形，即使使用实际的unicode字符的某些情况，提供一个清晰的注释会很有用。

示例：  
  
Example | Discussion
-|-
String unitAbbrev = "μs";|Best: perfectly clear even without a comment.
String unitAbbrev = "\u03bcs"; // "μs"|Allowed, but there's no reason to do this.
String unitAbbrev = "\u03bcs"; // Greek letter mu, "s"|Allowed, but awkward and prone to mistakes.
String unitAbbrev = "\u03bcs";|Poor: the reader has no idea what this is.
return '\ufeff' + content; // byte order mark|Good: use escapes for non-printable characters, and comment if necessary.
  
>Tip: Never make your code less readable simply out of fear that some programs might not handle non-ASCII characters properly. If that should happen, those programs are broken and they must be fixed.

提示: 不要让你的代码减少可读性仅仅因为害怕某些程序无法合适的处理非ASCII字符。如果发生了，只能说明那些程序是坏的，他们必须被修复。

# 3 Souce file structure
>A source file consists of, in order:  
1.License or copyright information, if present  
2.Package statement  
3.Import statements  
4.Exactly one top-level class  
Exactly one blank line separates each section that is present.  

一个源文件由以下组成，按顺序：
1. 许可及版权信息，如果存在的话
2. 包语句
3. 引用语句
4. 一个顶级类  
  
每个存在的部分之间只有一个空行

## 3.1 License or copyright infomation, if present
>If license or copyright information belongs in a file, it belongs here.如果许可或版权信息适合一个文件，那么它就属于这儿

## 3.2 Package statement
>The package statement is not line-wrapped. The column limit (Section 4.4, Column limit: 100) does not apply to package statements.   

包语句不换行。列数限制（4.4节，列数限制：100）不适用于于包语句

## 3.3 Import statements
### 3.3.1 No wildcard imports
>Wildcard imports, static or otherwise, are not used.通配符引用，无论是静态还是非静态，都不允许使用

### 3.3.2 No line-wrapping
>Import statements are not line-wrapped. The column limit (Section 4.4, Column limit: 100) does not apply to import statements.  

引用语句不换行。列数限制不适用于引用语句。

### 3.3.3 Ordering and spacing
>Imports are ordered as follows:  
1.All static imports in a single block.  
2.All non-static imports in a single block.  
If there are both static and non-static imports, a single blank line separates the two blocks. There are no other blank lines between import statements.  
Within each block the imported names appear in ASCII sort order. (Note: this is not the same as the import statements being in ASCII sort order, since '.' sorts before ';'.)  

引用语句按如下排序：
1. 一个包含所有的[静态引用](https://github.com/clover402/one-piece/blob/master/java/static%20import.md)语句块 
2. 一个包含所有非静态引用的语句块  
  
如果同时有静态和非静态引用，两个语句块之间要有一个空行。没有其他的空行在引用语句之间。  
在每个语句块内引用名按ASCII码顺序排序.(说明：这不等同于引用语句按ASCII排序，因为“.”排在“;”之前)

### 3.3.4 No static import for classes
>Static import is not used for static nested classes. They are imported with normal imports.  

静态引用不用于静态内嵌类。它们使用普通引用。

## 3.4 Class declaration
### 3.4.1 Exactly one top-level class declaration
>Each top-level class resides in a source file of its own.  

每一个顶级类存在于一个源文件中。

### 3.4.2 Ordering of class contents
>The order you choose for the members and initializers of your class can have a great effect on learnability. However, there's no single correct recipe for how to do it; different classes may order their contents in different ways.  
What is important is that each class uses some logical order, which its maintainer could explain if asked. For example, new methods are not just habitually added to the end of the class, as that would yield "chronological by date added" ordering, which is not a logical ordering.  

你为你的类成员和初始化块选择的顺序对可学习性有很大的影响。但是，对于怎么排序并没有准确的方法；不同的类可能按不同的方式排序。  
重要的是每个类都用了某种被问时可以说的清楚的逻辑进行排序。例如，新方法不只是随手加到类的尾部，因为这会导致按添加时间排序的顺序，这并不是一个有逻辑的顺序。

#### 3.4.2.1 Overloads: never split
>When a class has multiple constructors, or multiple methods with the same name, these appear sequentially, with no other code in between (not even private members).

当一个类有多个构造函数或者多个同名方法时，这些方法按顺序出现，在它们之间没有别的代码（甚至是一个私有成员）。


# 4 Formatting
>Terminology Note: block-like construct refers to the body of a class, method or constructor. Note that, by Section 4.8.3.1 on array initializers, any array initializer may optionally be treated as if it were a block-like construct.  

术语说明：块状结构指的是类、方法或者构造函数的内容。注意，依据 4.8.3.1关于数组初始化的内容，任何数组初始化块可以选择性的被作为一个块状结构。

## 4.1 Braces
### 4.1.1 Braces are used where optional
>Braces are used with if, else, for, do and while statements, even when the body is empty or contains only a single statement.

大括号用于if、else、for、do和while语句，甚至当方法体是空的或者只包含一个语句时。

### 4.1.2 Nonempty blocks: K & R style
>Braces follow the Kernighan and Ritchie style ("Egyptian brackets") for nonempty blocks and block-like constructs:  
* No line break before the opening brace.  
* Line break after the opening brace.  
* Line break before the closing brace.  
* Line break after the closing brace, only if that brace terminates a statement or terminates the body of a method, constructor, or named class. For example, there is no line break after the brace if it is followed by else or a comma.  

对于非空块和块状结构大括号尊重K&R风格：
* 在左括号之前不换行
* 在左括号之后换行
* 在右括号之前换行
* 在右括号之后换行，只有在括号结束了一个语句或者方法体、构造函数或者有名类时。例如，如果括号后面跟了else或者逗号，那么就没有换行

Examples:
```java
return () -> {
  while (condition()) {
    method();
  }
};

return new MyClass() {
  @Override public void method() {
    if (condition()) {
      try {
        something();
      } catch (ProblemException e) {
        recover();
      }
    } else if (otherCondition()) {
      somethingElse();
    } else {
      lastThing();
    }
  }
};
```
>A few exceptions for enum classes are given in Section 4.8.1, Enum classes.  

对于来自“4.8.1节(枚举类)”的枚举类有几个例外  

### 4.1.3 Empty blocks: may be concise(简明的)
>An empty block or block-like construct may be in K & R style (as described in Section 4.1.2). Alternatively, it may be closed immediately after it is opened, with no characters or line break in between ({}), unless it is part of a multi-block statement (one that directly contains multiple blocks: if/else or try/catch/finally).  

空的语句块或块状结构可以使用K&R风格(如4.1.2节描述的那样)。可选的，它也可以左括号之后马上跟上右括号，在两个括号（{}）之间没有字符和换行，除非它是多块状语句（一个包含了多个代码块的语句：if/else 或者 try/catch/finally）的一部分。  

Examples:
```java
  // This is acceptable
  void doNothing() {}

  // This is equally acceptable
  void doNothingElse() {
  }
  
  // This is not acceptable: No concise empty blocks in a multi-block statement
  try {
    doSomething();
  } catch (Exception e) {}
```
## 4.2 Block indentation: +2 spaces
>Each time a new block or block-like construct is opened, the indent increases by two spaces. When the block ends, the indent returns to the previous indent level. The indent level applies to both code and comments throughout the block. (See the example in Section 4.1.2, Nonempty blocks: K & R Style.)

每次一个新的代码块或者块状结构开始后，缩进要增加2个空格。当语句块结束时缩进要恢复原来的水平。缩进水平对于块中的代码和注释都有效。（见4.1.2节的示例，非空块：K&R风格。）

## 4.3 One statement per line
>Each statement is followed by a line break.  
每个语句后面要跟一个换行

## 4.4 Column limit: 100
>Java code has a column limit of 100 characters. A "character" means any Unicode code point. Except as noted below, any line that would exceed this limit must be line-wrapped, as explained in Section 4.5, Line-wrapping. 
>>Each Unicode code point counts as one character, even if its display width is greater or less. For example, if using fullwidth characters, you may choose to wrap the line earlier than where this rule strictly requires.

>Exceptions:  
1.Lines where obeying the column limit is not possible (for example, a long URL in Javadoc, or a long JSNI method reference).  
2.package and import statements (see Sections 3.2 Package statement and 3.3 Import statements).  
3.Command lines in a comment that may be cut-and-pasted into a shell.  

java代码有100个字符的列数限制。一个“字符”表示任意的unicode代码。除了下面特殊说明的外，任何超过这个限制的行都必须换行，就像4.5节-换行所说的那样。
>每个unicode码都算作一个字符，即使它的显示宽度或大或小。例如，如果使用全角字符，你可以选择比规则要求的更早些换行。 

例外:  
1. 所有的行都遵守列数限制是不可能的(例如,Javadoc里的长URL，或者一个长的JSNI方法引用).
2. 包语句和引入语句
3. 注释里一些会被剪切和粘贴到shell里的命令行

## 4.5 Line-wrapping
>Terminology Note: When code that might otherwise legally occupy a single line is divided into multiple lines, this activity is called line-wrapping.  
There is no comprehensive, deterministic formula showing exactly how to line-wrap in every situation. Very often there are several valid ways to line-wrap the same piece of code.

术语说明：当原先占据一行的代码被分成了多行，这个活动被称为换行。  
没有详细的固定的公式来表明每种情况具体该怎么换行。通常对于同样的代码有几种合法的换行方式。

>Note: While the typical reason for line-wrapping is to avoid overflowing the column limit, even code that would in fact fit within the column limit may be line-wrapped at the author's discretion.

说明：典型的换行理由是避免超过列数限制，甚至代码事实上在限制以内也可能由于作者的意图而换行

>Tip: Extracting a method or local variable may solve the problem without the need to line-wrap.  

提示：提前一个方法或者局部变量可以不用换行而解决这个问题。

### 4.5.1 Where to break
>The prime directive of line-wrapping is: prefer to break at a higher syntactic level. Also:  
1.When a line is broken at a non-assignment operator the break comes before the symbol. (Note that this is not the same practice used in Google style for other languages, such as C++ and JavaScript.)  
This also applies to the following "operator-like" symbols:  
*the dot separator (.)  
*the two colons of a method reference (::)  
*an ampersand in a type bound (<T extends Foo & Bar>)  
*a pipe in a catch block (catch (FooException | BarException e)).  
2.When a line is broken at an assignment operator the break typically comes after the symbol, but either way is acceptable.  
This also applies to the "assignment-operator-like" colon in an enhanced for ("foreach") statement.  
3.A method or constructor name stays attached to the open parenthesis (() that follows it.  
4.A comma (,) stays attached to the token that precedes it.  
5.A line is never broken adjacent to the arrow in a lambda, except that a break may come immediately after the arrow if the body of the lambda consists of a single unbraced expression. Examples:  
```java
MyLambda<String, Long, Object> lambda =
    (String label, Long value, Object obj) -> {
        ...
    };

Predicate<String> predicate = str ->
    longExpressionInvolving(str);
```
>Note: The primary goal for line wrapping is to have clear code, not necessarily code that fits in the smallest number of lines.

换行的主要指导是：尽量在一个更高级别的语法处换行。也就是说：
1. 当一个行在非赋值操作符处换行时换行要在符号之前。（注意这不同于使用谷歌风格的其他语言，例如C++和JavaScript）  
这也用于如下类操作符符合：
* 点分隔符(.)
* 双冒号方法引用(::)
* 类型约束中的&(<T extends Foo & Bar>)
* catch块中的管道符号(catch( FooException| BarException e))
2. 当一行以赋值运算符换行时一般来说换行实在符号后面，但是另外一种方式也是可以接受的。  
这个适用于加强版的for（foreach）语句中“类复制运算符”的冒号
3. 一个方法或者构造函数后面会紧跟一个左括号（(）
4. 一个逗号（,）紧跟在它前面的内容之前
5. 一行绝对不会在lambda表达式中箭头附近换行，换行可以在箭头之后马上出现除非lambda表达式的方法体由一个没括号的单个表达式组成  

说明: 换行主要的目标是拥有清晰的代码，而不是保持最小行数的代码

### 4.5.2  Indent continuation lines at least +4 spaces
>When line-wrapping, each line after the first (each continuation line) is indented at least +4 from the original line.  
When there are multiple continuation lines, indentation may be varied beyond +4 as desired. In general, two continuation lines use the same indentation level if and only if they begin with syntactically parallel elements.  
Section 4.6.3 on Horizontal alignment addresses the discouraged practice of using a variable number of spaces to align certain tokens with previous lines.

当换行时，第一行后的每行（后续行）相较原来的行至少要增加4个空格的缩进  
当有多个后续行时，缩进可能会根据需要大大超过4个空格。一般来说，2个后续行使用同样的缩进水平当且仅当他们以语法上相识的元素开始时  
4.6.3节关于水平对齐的内容表明不鼓励使用一个变化数量的空格来对齐之前行的某些内容。

## 4.6 Whitespace
### 4.6.1 Vertical Whitespace
>A single blank line always appears:  
1..Between consecutive members or initializers of a class: fields, constructors, methods, nested classes, static initializers, and instance initializers.  
&nbsp;&nbsp;*Exception: A blank line between two consecutive fields (having no other code between them) is optional. Such blank lines are used as needed to create logical groupings of fields.  
&nbsp;&nbsp;*Exception: Blank lines between enum constants are covered in Section 4.8.1.  
2..As required by other sections of this document (such as Section 3, Source file structure, and Section 3.3, Import statements).  
A single blank line may also appear anywhere it improves readability, for example between statements to organize the code into logical subsections. A blank line before the first member or initializer, or after the last member or initializer of the class, is neither encouraged nor discouraged.  
Multiple consecutive blank lines are permitted, but never required (or encouraged).  

一个空行通常出现在：
1. 一个类的连续成员或者初始化块之间：成员变量、构造函数、成员方法、内嵌类、静态初始化块和直接初始化
* 例外：两个连续成员变量间的空行是可选的。这类空行是为了创建成员变量的逻辑分组而使用
* 例外：枚举常量间的空行会在4.8.1节里面说明
2. 根据此文档的其他章节所需（例如第三章，源文件结构，3.3节，导入语句）  

一个空行可以出现在任意可以增加可读性的位置，例如在组织代码到多个逻辑块的语句间。类中在第一个成员或初始化语句之前的空行，或者是最后一个成员或者初始化语句之后的空行既不鼓励也不反对。  
多个连续空行是允许的，但是不需要（不鼓励）

### 4.6.2 Horizontal whitespace
>Beyond where required by the language or other style rules, and apart from literals, comments and Javadoc, a single ASCII space also appears in the following places only.  
1..Separating any reserved word, such as if, for or catch, from an open parenthesis (() that follows it on that line  
2..Separating any reserved word, such as else or catch, from a closing curly brace (}) that precedes it on that line  
3..Before any open curly brace ({), with two exceptions:  
&nbsp;&nbsp;*@SomeAnnotation({a, b}) (no space is used)  
&nbsp;&nbsp;*String[][] x = {{"foo"}}; (no space is required between {{, by item 8 below)  
4..On both sides of any binary or ternary operator. This also applies to the following "operator-like" symbols:  
&nbsp;&nbsp;*the ampersand in a conjunctive type bound: <T extends Foo & Bar>  
&nbsp;&nbsp;*the pipe for a catch block that handles multiple exceptions: catch (FooException | BarException e)  
&nbsp;&nbsp;*the colon (:) in an enhanced for ("foreach") statement  
&nbsp;&nbsp;*the arrow in a lambda expression: (String str) -> str.length()  
but not  
&nbsp;&nbsp;*the two colons (::) of a method reference, which is written like Object::toString  
&nbsp;&nbsp;*the dot separator (.), which is written like object.toString()  
5..After ,:; or the closing parenthesis ()) of a cast  
6..On both sides of the double slash (//) that begins an end-of-line comment. Here, multiple spaces are allowed, but not required.  
7..Between the type and variable of a declaration: List<String> list  
8..Optional just inside both braces of an array initializer  
  &nbsp;&nbsp;new int[] {5, 6} and new int[] { 5, 6 } are both valid  
9..Between a type annotation and [] or ....  
  This rule is never interpreted as requiring or forbidding additional space at the start or end of a line; it addresses only interior space.  
  
超越了语言或者其他风格规则，除了字面值、注释和Javadoc外，一个ASCII空格只会出现在如下地方：
1. 分隔任何保留字，例如if、for或者catch，与这行中跟在它后面的左小括号（(）
2. 分隔任何保留字，例如else或catch，与这行中在它之前的右大括号（}）
3. 在任何左大括号之前（{），但又2个例外：
* @SomeAnnotation({a,b}) (注解后面的大括号不用加空格)
* String[][] x = {{"foo"}};(二维数组初始化时的两个大括号之间不用加空格)
4. 在任意二元或三元运算符的两边。这也适用于下面类似运算符的符号：
* 在类型限定的连接符&：<T extends Foo & Bar>
* 处理多个异常的catch语句块中的管道符号|：catch(FooException | BarException e)
* 在加强版for语句（foreach）中的冒号：
* lambda表达式中的箭头：(String str) -> str.length()  

不适用于如下：
* 表示方法引用的两个冒号::，应该这样写Object::toString
* 点运算符（.），应该这样写object.toString()
5. 在逗号冒号和分号（,:;）后面，或者强制类型转换的右括号（)）
6. 在双斜杠（//）的两边


