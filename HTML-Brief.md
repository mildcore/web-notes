



# HTML - HyperText Markup Language



## HTML Basic

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>我的测试站点</title>
  </head>
  <body>
    <p>这是我的页面</p>
  </body>
</html>


# 元素
# one element with opening-tag、content、closing-tag
<p>content</p>

# Block and inline elements
# 块级元素和内联元素
<p>		# block-level element
<em>	# inline element

# 空元素（Empty elements）有时也被叫作 void elements. 可以只有一个开始标签
<img src="..">

    
# 属性，单双引号皆可，有时可省略引号
<a href=".." title=".." target="_blank">site</a>
    
# 布尔属性，属性只有一个值可省略disabled="disabled"
<input type="text" disabled>	

    
# 实体引用 entity references
<	&lt;
>	&gt;
"	&quot;
'	&apos;
&	&amp;
    
    &#nnnn;	 # decimal unicode code point
    &#xhhhh; # hexadecimal
    
# 注释
<!-- 我在注释内！ -->
    
```



## HEAD

```html
# html metadata 元数据
<head></head>

# title
<title>document title</title>			# title of html document, metadata
# don't get confused with `h1`: 
<h1>title of one page content</h1>		# title of page, not a metadata


# the `meta` element
# charset
<meta charset="utf-8">		

# name='' content=''
<meta name="author" content="..">
<meta name="description" content="..">
-<meta name="keywords" content="..">		# keywords : not used anymore.

# open graph data (a metadata protocol)
<meta property="og:image" content="..">
<meta property="og:description" content="..">
<meta property="og:title" content="..">
..

# twitter cards
<meta name="twitter:title" content="..">
..


# custom icons: used in browser tab、bookmarks panel 
# favicon (favorites icon): 16-pixel square icon. 
<link rel="icon" href="favicon.ico" type="image/x-icon">
# other icon type
<link rel="apple-touch-icon-precomposed" href=".." size="">
<link rel="shortcut icon" href="..">


# css
<link rel="stylesheet" href="my-css-file.css">

# js
# could be put just before </body> to ensure html content ready. 
# or with defer (also for speed up)
<script src="my-js-file.js"></script>
<script src="my-js-file.js" defer></script>		# not worked in old browser.


# primary language of the html document. 
# seo, accessibility..
<html lang="en-US">
<html lang="zh-CN">
    
```





## Text

```html
# html give text structure and meaning(aka. semantics语义)
structure:
  1. better for read
  2. better for seo
  3. benefit screen-reader (for Severely visually impaired people)
  4. for css/js target
semantics:
  1. giving our content the correct meaning, function, or appearance
  2. accessibility 可访问性
  3. SEO
  

# 段落paragraph
<p></p>

# 标题headings
<h1> .. <h6>

# 有序列表ol、无序列表ul、Nesting lists
<ol>
  <li></li>
</ol>

# Emphasis 强调，Strong importance 着重强调
<em>		# ofen italic
<strong>	# ofen bold

# 无语义表象元素（presentational elements），Italic, bold, underline
# 不利于accessibility
<i> 外国文字，分类名称，技术术语，一种思想……
<b> 关键字，产品名称，引导句……
<u> 专有名词，拼写错误……

```



## Hyperlinks

```html
<a href="url" title='提示'>

# 片段
<a href="#fragment">
<h1 id="fragment"></h1>

# 下载链接
<a href="url" download='file.name'>

# 邮件链接 mailto:
<a href="mailto:地址">
<a href="mailto:地址?cc=抄送&bcc=密送&subject=主题">

```



## Advanced text formatting

```html
# 1.描述列表description lists
<dl>
  <dt>A</dt><dd>a</dd>
</dl>


# 2.引文quotations
# Blockquotes
<blockquote cite=''> </blockquote>

# Inline quotations
<q cite=''>引文</q>

# Citations  
<a href=''>
  <cite>引文</cite>
</a>


# 3.Abbreviations 缩写
<abbr title="全称"></abbr>

# 4.contact details 联系方式
<address>

# 5.Superscript and subscript 上下标
<sup> 、<sub>

# 6.computer code 代码
<code>: 计算机通用代码，不保留空白字符
<pre>: 保留空白字符
<var>: 变量名
<kbd>: 电脑键盘（或其他）输入
<samp>: 计算机程序的输出。

# 7.times and dates 时间日期
<time datetime="2016-01-20">20 January 2016</time>

```



## Document and website structure

```html
### structuring content 结构化内容
**header**、 **navigation bar**、 **main content**、 **sidebar**、 **footer**
<header>：页眉。
<nav>：导航栏。
<main>：主内容(only one)。可以有各种子内容<article>、<section> 和 <div> 等。
<aside>：侧边栏，经常嵌套在 <main> 中。
<footer>：页脚。


### layout elements 布局元素
**non-semantic element 无语义元素** 
`<span>` is an inline 
`<div>` is a block level 

**Line breaks and horizontal rules 换行与水平分割线** 
`<br>` 换行 line break
`<hr>`水平线 create a horizontal rule


### Planning a simple website 规划网站
1. 通用内容、页眉、页脚
   header: title\logo
   footer: copyright notice、contact details; 
       	   terms、condition; 
           language choose; 
           accessibility policy;
2. 页面结构草图
   header
   content   sidebars
   footer
3. 内容列表
4. 卡片分类 card sorting
5. 站点地图 bubble & line
   bubble for each page，lines for typical workflow between pages
    
```