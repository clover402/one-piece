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






