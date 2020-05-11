

[TOC]

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
<a href="url" title='提示' target='_blank'>

# 片段
<a href="#fragment">
<h1 id="fragment"></h1>

# 下载链接
<a href="url" download='file.name'>

# 邮件链接 mailto:
<a href="mailto:地址">
<a href="mailto:地址?cc=抄送&bcc=密送&subject=主题">

```



## Advanced Text Formatting

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



## Document and Website Structure

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



## Images

```html
# 图像 
alt 			# 备选文字（图片无法正常获取时）
title			# 悬停提示
width、height	# 图片未加载时预留空间
<img src="images/dinosaur.jpg" alt="恐龙" width="400" height="341" title="T-Rex">

# 使用 figure 和 figcaption 为图像附加说明而不是 div 和 p
<figure>
    <img src="..">
    <figcaption>..</figcaption>
</figure>

# 无语义装饰图片可用 css 实现
# 以下为页面中的所有段落设置一个背景图片
p {
  background-image: url("images/dinosaur.jpg");
}

```



## Video and Audio Content

```html
# video .mp4 .webm
	`src`
	`controls`							  # browser's own control interface
	paragraph inside the video tags		   	# fallback content
<video src="rabbit320.webm" controls>
  <p>你的浏览器不支持 HTML5 视频。可点击<a href="rabbit320.mp4">此链接</a>观看</p>
</video>

# 多格式支持 `source[src、type]` 代替 `src`
# 使用type帮助浏览器快速判断
<video controls>
  <source src="rabbit320.mp4" type="video/mp4">
  <source src="rabbit320.webm" type="video/webm">
  <p>你的浏览器不支持 HTML5 视频。可点击<a href="rabbit320.mp4">此链接</a>观看</p>
</video>

# The new features of video:
width and height	# 视频还是会自己保持纵横比 aspect ratio
autoplay
loop
muted
poster		# 预览图像
preload		# 预加载控制 none、auto、metadata[仅元数据]


# audio .mp3 .ogg
# 基本与video一致, 不支持width height poster
<audio controls>
  <source src="viper.mp3" type="audio/mp3">
  <source src="viper.ogg" type="audio/ogg">
  <p>Your browser doesn't support HTML5 audio. </p>
</audio>


# 视频文字轨道 track[kind, src, srclang]
WEBVTT (Video Text Tracks)  .vtt
kind:
    subtitles : 翻译字幕
    captions : 字幕
    descriptions : 被播放器读出来的内容（针对视力障碍人士）
<video controls>
    <source src="example.mp4" type="video/mp4">
    <source src="example.webm" type="video/webm">
    <track kind="subtitles" src="subtitles_en.vtt" srclang="en">
</video>

# Tools for V/A: Miro Video Converter 和 Audacity

```

## iframe 、object、embed

```html
# iframe 用来嵌入web document
[src, height, width, allowfullscreen, frameborder, sandbox]
`frameborder`   # 默认1，设为0后没有边框
`src`           # 使用js在主内容加载完后再设置src, 可提高网站加载速度
备选内容 		# fallback应变内容, 浏览器不支持时显示
<iframe src=".." width="100%" height="500" frameborder="0" allowfullscreen sandbox>
  <p> Fallback </p>
</iframe>

# iframe安全问题
# iframe是个广泛的攻击目标（attack vector）. 比如clickjacking
# tips
1. Only embed when necessary
2. use https
	HTTPS reduces the chance that remote content has been tampered with in transit,
	HTTPS prevents embedded content from accessing content in your parent document, and vice versa.
3. use sandbox
    sandbox="" 中添加允许的权限，默认全部限制。不要同时使用allow-scripts, allow-same-origin
4. CSP 内容安全策略
    配置X-Frame-Options限制其他网站对本站的iframe调用, 3种取值
    "deny"
    "sameorigin"
    "allow-from [uris]"


# <embed/>和<object/> —— 用来嵌入多种类型的外部内容的通用嵌入工具
# 插件 : 一种对浏览器无法原生读取的内容提供访问的软件
src				data
type			..    	   			
height			..		
width			..
ad-hoc attrs	 <param/>          # 插件参数
不支持			  after <param/>    # fallback html content
<object data="mypdf.pdf" type="application/pdf" width="800" height="1200" typemustmatch>
  <p>no PDF plugin</p>
</object>

```



##  SVG

```html
# SVG 矢量图象 Scalable Vector Graphics

# img引入 缺点：无法js操作，无法外部css控制
<img src="xx.svg" alt=".." height="87px" width="100px" />
# 兼容老版，svg放到`srcset`
<img src="xx.png" srcset="xx.svg">

# 内联svg (SVG inline, inlining SVG), svg图像数据(xml)直接插入到html
# 支持js，css等的很多操作
<svg width="300" height="200">
    <rect width="100%" height="100%" fill="green" />
</svg>

# iframe引入 注意：js操作必须同源, 浏览器完全不支持iframe才会显示备用img
<iframe src="triangle.svg" width="500" height="500" sandbox>
    <img src="triangle.png" alt="Triangle with three unequal sides" />
</iframe>

```

## Responsive Images

```html
# 响应式图片 
img[srcset sizes]
picture {source[srcset type media],
		img}

# 1.不同尺寸、不同分辨率 srcset sizes
# (max-width: 480px) 440px #  view-port小于480px时，使用宽度440px, 并选择最接近的图像
# src 兼容老版
<img srcset="elva-fairy-320w.jpg 320w,		
             elva-fairy-480w.jpg 480w,		
             elva-fairy-800w.jpg 800w"		
     sizes="(max-width: 320px) 280px,
            (max-width: 480px) 440px,	
            800px"
     src="elva-fairy-800w.jpg" alt="Elva dressed as a fairy">

# 强制移动浏览器使用真实视窗宽度
<meta name="viewport" content="width=device-width">

# 2.相同尺寸、不同分辨率 srcset
# 标准低分辨率设备standard/low		(一个设备pixel表示一个CSS pixel) 加载第一个图像 1x
# 高分辨率设备如打印机 Nx	           (多个设备pixel表示一个CSS pixel)
img {width: 320px;}
<img srcset="elva-fairy-320w.jpg,
             elva-fairy-480w.jpg 1.5x,
             elva-fairy-640w.jpg 2x"
     src="elva-fairy-640w.jpg" alt="Elva dressed as a fairy">

# 3.美术设计问题 Art Direction Problem
# 设计不同剪裁的图片，以在不同尺寸设备获取更好的体验
# picture[media, srcset] media指定一个条件
<picture>
  <source media="(max-width: 799px)" srcset="elva-480w-close-portrait.jpg">
  <source media="(min-width: 800px)" srcset="elva-800w.jpg">
  <img src="elva-800w.jpg" alt="Chris standing up holding his daughter Elva">
</picture>

# 4.source使用type属性, 浏览器可以快速选择支持的格式
<picture>
  <source type="image/svg+xml" srcset="pyramid.svg">
  <source type="image/webp" srcset="pyramid.webp"> 
  <img src="pyramid.png" alt="regular pyramid built from four equilateral triangles">
</picture>

```



## Table

```html
# 1.
# table tr td
# th替换td来表示一个标题单元格
# colspan、rowspan
<td colspan=2 rowspan=3/> 指定单元格行列长度 

# 2.定义整个列的样式
# colgroup col[span]
<table>
    <colgroup>
      <col>											# 列0不设置
      <col style="background-color: yellow" span="2">	# span指定同时设置列1、2
    </colgroup>
    <tr>
        <th>head 0</th>
        <td>cell 1</td>
        <td>cell 2</td>
    </tr>
</table>

# 3.table标题
<caption></caption>

# 4.table结构 thead tbody tfoot
# 标题行放到 thead
# 总结行放到 tfoot, 会自动显示到tbody后面

# 5.table嵌套

# 6.accessibility
# 6.1 th[scope: col|row|colgroup|rowgroup]  指明一个单元格是列的标题，或者行的标题
<tr>
  <th scope='col'></th>
  <th scope='col'></th>
</tr>
<tr>
  <th scope='row'></th>
  <td></td>
</tr>
# 6.2 th[id] td[headers]
<th id='a'></th> 
<th id='b'></th>
<td headers='a b'></td>

```



## Forms

```html
<form action="URI" method="post"></form>

# 文本输入
# label input textarea
# input定义默认值value="", textarea直接写在content部分
# 表单内容控件定义name="", 提交时作为json数据的key
<div>
    <label for="name">Name:</label>		# label应使用for, 值=对应控件的id
    <input type="text" id="name" >		# input的type属性有关表单验证， text、email、password
</div>
<div>
    <label for="msg">Message:</label>
    <textarea id="msg"></textarea>		# textarea不是空标签
</div>

# 按钮控件 button
<button type='submit'>按钮</button>	   # type: submit、reset、button
# input只能实现纯文本按钮
<input type="submit">				    # submit、reset、button, 浏览器会提供默认按钮文字


# filedset legend
# 实现单选按钮组，等等
<fieldset>							   
    <legend>Fruit juice size</legend>	 # 控件组说明
    <p>
        <input type="radio" name="size" id="size_1" value="small">	# 这是一组name="size"的radio
        <label for="size_1">Small</label>
    </p>
	<p>
      <input type="radio" name="size" id="size_2" value="medium">
      <label for="size_2">Medium</label>
    </p>
</fieldset>

# label用for和控件id连起来后，可以点击响应


# 表单结构
# 可结合h1、h2, section, fieldset, p, div, li等等实现一个结构化的表单结构 
<form>
    <h1>表单主题</h1>
    <section>
        <h2>用户信息模块</h2>
        <fieldset>性别选择，比如单选按钮用无序列表包含.. </fieldset>
        <p>姓名输入..</p> 
    </section>
    <section>
        比如支付信息模块..
    </section>
    <div><button type='submit'>按钮</button></div>
</form>

```



## Basic Native Form Ctrls

```html
# common attributes
name、value  # 即kw
autofocus	# 自动聚焦
disabled	# 禁用
form	    # don't use

-------------- input ----------------
# 单行文本输入框 input
readonly         # 只读
disabled         # 禁用后不作为数据
placeholder      # 提示文字
size             # 宽度
maxlength        # 最长输入
<input type="text">				# 普通单行文本
<input type="email" multiple>	 # 邮件输入框  mutiple可接受多个邮件，逗号隔开
<input type="password">		     # 密码框
<input type="search">		     # 搜索框
<input type="tel">			     # 电话
<input type="url">			     # url
<input type="hidden">			 # 隐藏, 仍可发送数据

# 多行文本输入框 textarea
<textarea cols="30" rows="10"></textarea>
cols	20	    文本控件的可见字符宽度，默认20
rows		    控制的可见文本行数
wrap	soft	超出宽度后换行模式 hard 或 soft

# 单选框、复选框 radio checkbox
# checked默认选中
<input type="radio" checked id="soup" name="meal">
<input type="checkbox" checked id="carrots" name="carrots" value="carrots">

# number  
min, max, step
<input type="number" name="age" id="age" min="1" max="10" step="2">
# range
min, max, step
<input type="range" name="beans" id="beans" min="0" max="500" step="10">

# 时间
<input type="datetime-local">
<input type="date">
<input type="time">
<input type="month">
<input type="week">
...

# color 拾色器
<input type="color">

# file 文件选择
accept multiple
<input type="file" accept="image/*" multiple>

# 图像按钮
# 发送图像坐标值（左上角为0，0） 'name'.x 'name'.y
# http://foo.com?pos.x=12&pos.y=23
<input type="image" alt="Click me!" src="my-img.png" width="80" height="30"/>



# 下拉菜单
# 选择框
# select{option..}
<select id="simple" name="simple">
  <option>Banana</option> 
  <option selected>Cherry</option> 	# 可通过selected指定默认选择
</select>
# optgroup 对 option分组
<select id="groups" name="groups">
  <optgroup label="fruits">
    <option>Banana</option>
	...
  </optgroup>
</select>
# 实现多选 select[multiple]
<select multiple></select>

# datalist 自动补全下拉菜单
# 控件属性list引用一个datalist
<input type="text" name="fruit" id="myFruit" list="mySuggestion">
<datalist id="mySuggestion">
  <option>Apple</option>
  ...
</datalist>

# 向后兼容
# datalist中可以是一个select, 在浏览器不支持datalist时会显示选择框, 反之会忽略非option元素



# 进度条
<progress max="100" value="75">75/100</progress>	# fallback文本: 75/100

# 仪表 meter[min low high max optimum]
<meter min="0" max="100" value="75" low="33" high="66" optimum="50">fallback</meter>
# 值范围划分为三个部分：
    [min, low]		scope1
    (low, high)		scope2
    [high,max]		scope3
# optimum 最优值, 根据optium的位置, 依次决定3个scope的性质
	scope1		good    -     bad
	scope2		  -    good	   -
	scope3		bad	    -     good
# 浏览器根据当前值来改变米尺的颜色。
    good  绿色
     -    黄色
    bad   红色

```

## Sending Form Data

```html
# form [action method]
action: url
method: GET、POST..

# 此外，GET是一个幂等操作idempotent
# 幂等idempotent: 多次操作（参数相同）不会造成额外影响; 返回值不一定相同; 比如删除操作, abs()
# 对于HTTP Method有: safe操作GET/HEAD/OPTIONS, 非safe的PUT/DELETE

# GET
	没有body(empty), 数据追加到url
	数据长度受限
	不安全，不适合敏感数据
GET /?say=Hi&to=Mom HTTP/1.1
Host: www.foo.com

# POST
	数据追加到body
POST / HTTP/2.0
Host: foo.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 13

say=Hi&to=Mom

# 发送文件POST
enctype
	application/x-www-form-urlencoded      # default
	multipart/form-data			          # file
	text/plain					     	 # html5 debugging
<form method="post" enctype="multipart/form-data"></form>

# 安全问题 来源于Server端数据处理方式, 而不是表单本身
# Tips: Never trust users, including yourself
  1. Escape potentially dangerous characters.
  2. Limit the incoming amount of data to allow only what's necessary
  3. Sandbox uploaded files. 将文件存放在不同server, 通过子域名或不同域名访问
XSS		针对用户
CSRF	针对服务端	CSRF攻击类似于XSS攻击，相同的方式——向Web页面中注入客户端脚本
SQL注入			            # 针对数据库
HTTP数据头注入和电子邮件注入     # 会话劫持、网络钓鱼


```

