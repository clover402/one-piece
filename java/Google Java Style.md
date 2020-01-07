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
1. 一个包含所有的静态引用[^1] 语句块  
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



[^1]: 详情参考[静态引用](http://www.baidu.com)
