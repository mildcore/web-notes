[TOC]

## 标题

1-6级标题, 允许在结尾加上任意的结束#标记

```
#
##
###
####
#####
###### 标题6 ###  // 结束标记可选
```
1级标题和2级标题的另外表示

```
1级标题
=======		// 3个以上=作为下划注解
2级标题
-------	    // 3个以上-
```



## 段落

1. 一个段落是连续的行，段落之间用空行分开；
2. 同一个段落的行之间可以使用硬换行实现分行，即两个空格加普通换行；
```
paraA  
A1 
A2

paraB
```
paraA  
A1 
A2

paraB



## 引用

可以嵌套引用，包含多个段落，包含列表等

```
> 引用1
> 段落1 
>
>> 嵌套引用
>
> 引用段落2
> 
> 1.   This is the first list item.
> 2.   This is the second list item.
> 
>     CODE IS CODE	// 插入代码块必须5个空格，因为>后面跟着的第1个空格是引用语法的一部分.
```



## 列表
无序列表 `* - +`  
```
* one
* two
```
有序列表，转换后是li，所以实际显示数字不受输入数字影响
```
1. one
2. two
```
列表中每一项可以包含多个段落，每个段落必须缩进4个空格或一个tab
```
1.  This is a list item with two paragraphs. Lorem ipsum dolor
    sit amet, consectetuer adipiscing elit. Aliquam hendrerit
    mi posuere lectus.

    Vestibulum enim wisi, viverra nec, fringilla in, laoreet
    vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
    sit amet velit.

2.  Suspendisse id sem consectetuer libero luctus adipiscing.
```

列表中包含引用

```
*   A list item with a blockquote:
		 							// 个人建议这里的空行不要省略，不然生成的html标签可能很奇怪 
    > This is a blockquote
    > inside a list item.
```
列表中包含代码块，必须缩进8个空格或2个tab
```
*   A list item with a code block:

        <code goes here>
```
为了避免无意生成的列表，输入数字时需要注意，可以对.使用转义
```
1991\. 阻击他生成列表,加个反斜杠.
```



## 代码
代码块 4个空格或一个tab, 与段落之间必须用一个空行分开
```
This is a normal paragraph:

    This is a code block.
```
code span
```
`code` 或
`` `code ``   // 前后可以有一个空格，以实现输出`
```



## 强调
```
*single asterisks*    em
_single underscores_  em

**double asterisks**    strong
__double underscores__  strong
```



## 水平线
```
3个以上*或-
* * *
***
*****
- - -
---------------------------------------
```
- - -



## Link
### 内联式与引用式链接
inline
```
[an example](http://example.com/ "Title")
```
reference 大小写不敏感，可以使用默认id
```
[an example] [id]

[id]: http://example.com/  "Optional Title"  //双引号可以为换成单引号、括号


默认id:
[Google site][]

[google site]: www.google.com
```
### 简单链接
```
<http://example.com/>

<address@example.com>   // 针对邮箱地址生成html时，会被编码为
<a href="&#x6D;&#x61;../a> 的形式，以减少垃圾邮件地址收割机的影响
```



## 图片

与link相似，仅仅加上！
```
![an example](http://example.com/ "Title")
```



## 转义`\`
```
\   backslash
`   backtick
*   asterisk
_   underscore
{}  curly braces
[]  square brackets
()  parentheses
#   hash mark
+   plus sign
-   minus sign (hyphen)
.   dot
!   exclamation mark
```



## 关于HTML
### 内联HTML

如果在markdown里直接使用html标签，对于block标签中，是不能再嵌套使用markdown语法的，inline标签则可以。

### HTML自动转义
<  & 

只会转义必要的部分  
&copy;  源码： `&copy;` &不会被转义  
AT&T 源码： `AT&T` &会被转义成&amp;

当然如果在code块或span里，都会转义



## Extras
### table
[GFM](https://docs.github.com/en/github/writing-on-github/organizing-information-with-tables)
```markdown
| First Header  | Second Header |
| :-----------: | ------------: |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |
```



## 其他
### python实现
[markdown2](https://github.com/trentm/python-markdown2/wiki/Extras)

markdown2扩展
* code-friendly
* fenced-code-blocks
* footnotes
* strike
* target-blank-links
* tables
* tag-friendly : 标题#后要求有一个空格

针对admin
* header-ids
* toc

针对user
* nofollow

### 参考

[原始 Markdown][1]

[1]: https://daringfireball.net/projects/markdown/syntax "markdown syntax"