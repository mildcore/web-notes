# CSS - Cascading Style Sheets



## CSS-Basic

```css

应用css的方法
# 1.外部样式表 
<link rel="stylesheet" href="styles.css">
# 2.内部样式表
<style></style>
# 3.内联样式    
定义在元素的style属性里, 前两个定义在HTML的head里
<h1 style="color: blue;border: 1px solid black;">Hello World!</h1>

基本语法
选择器 property:value 花括号 冒号 分号
h1 {
    color: red;			
    font-size: 5em;
}
声明 声明块 规则/规则集

基本属性和值
color: red 
font-size: 20px
font-weight: bold
width: 20px				# 值可以应用函数如calc(%90-20px)
border: none
text-decoration: none


# Selector
# 元素选择器 	p, li {color: green;}
# 类选择器 		.classname
# 元素.类选择	p.classname, li.classname

# Combinator
根据位置选择（元素组合）
# 包含选择器 descendant combinator
li em		# li下的em
# 相邻选择器 adjacent sibling combinator
h1 + p		# 与h1相邻同层级的p, 之间不能有其他任何元素

# 根据状态选择（元素状态）
a:link	    # 超链接未访问
a:visited	# 访问过的
a:hover		# 鼠标悬停

# 复杂选择器
body p + li .em		# 空格表示子元素， +表示相邻同级元素, li .em之间有空格含义就不一样了


# 选择器的特性
专一性 Specificity
级联规则和专用规则
级联Cascade: 		同样的选择器定义，应用后面的定义
专用Specificity:	更专有的选择器会被应用，如类选择器比元素选择器更专有


@规则
# 导入一个样式表
@import 'styles2.css';
# 媒体查询media query, 根据条件成立时才应用样式
@media (min-width: 30em) {
    body {background-color: green;}
}


速记属性 shorthands properties 
# font, background, padding, border, margin
一行里设置多个值  
注意: 省略的属性会重置会原始值 
padding: 10px 15px 15px 5px;   
= 同时设置了padding-top, padding-right, padding-bottom, padding-left
background: red url(bg-graphic.png) 10px 10px repeat-x fixed;
=
background-color: red;
background-image: url(bg-graphic.png);
background-position: 10px 10px;
background-repeat: repeat-x;
background-attachment: fixed;

注释
/* comments */

空白字符 
大部分被忽略


```



## How CSS Works

```css
LOAD HTML -> Parse HTML --->  DOM Tree	-----------------------> Display
			   ↓               ↑ attach style to nodes
			LOAD CSS  -----> Parse CSS
# FLOW
1.load html
2.parse html to dom
3.load resourses (js,img,css..)
4.parse css. sort rules to `buckets`. work out and attach style to dom-nodes(called a render tree)
5.The render tree is laid out in the structure it should appear in after rules applied to it.
6.visual display (painting)

# DOM : has a tree-like structure
# DOM Node: element, attribute, and piece of text

# CSS会跳过不理解的规则 selector property value..
# 利用这一特性实现对新旧版本浏览器的同时支持
.box {
  width: 500px;				   # 相当于一个fallback
  width: calc(100% - 50px);		# 浏览器如果不支持就忽略, 支持的浏览器则应用这条
}

```



## Cascade, Specificity and Inheritance

```css
Cascade: 层叠  具有相同Specificity的规则，应用后面的规则(在Speci之前要看Origin Cascade Order)
Specificity: 专用性、特定性  最相关原则，相当于优先级(指向同一元素的不同选择器专用性可能不同)
Inheritance: 继承  有些属性从父元素继承，有些不会(继承属性和非继承属性)


#3Factor [DESC]: or 2 [先看Cascade Algorithm -> Specificity Algorithm]
Importance
Specificity
Source order: 相同权重，应用后面的规则


#Cascade （algorithm） applied before the specificity algorithm
# which participate in the cascade:
1. css-declaration				 # property/value pairs
2. @ 有declaration			     # @media{}等，也是以declaration参与cascade
3. @[entities] 没有declaration     # @font-face作为整体参与cascade
4. @keyframes{}					 # 特例，仍作为整体
5. @import ; @charset	          # 不受级联算法影响（cascade algorithm)   
# cascade origin
1.user-agent stylesheets (browser basic stylesheets)
    注意区分browser basic stylesheet与browser default stylesheet(用于initial取值)
    不同的浏览器可能有区别，因此开发常通过一个reset style sheet来得到一个确定的初始状态 
2.user stylesheets
3.author stylesheets
# cascade order [ASC]:
!important的3个origin顺序刚好相反，mdn中文翻译错了
    Origin		Importance
1	user-agent	normal
2	user		normal
3	author		normal
4	animations	@keyframes{}
5	author		!important
6	user		!important
7	user-agent	!important
8	transitions
 

#Specificity (algorithm)
计算一个选择器的专用性值，累加
千	内联样式
百	ID选择器，每个记一个数
十	类、属性、伪类
个	元素、伪元素


#Inheratance
1. 继承的传递性
2. 控制继承
inherit  强制继承（不管是不是继承属性）
initial  浏览器默认样式取值。如果browser default stylesheet未设置`且该属性是自然继承的`，则继承
		    [对于非继承属性，应用默认值]
		    [对于继承属性，应用的是默认值（优先）或者继承值，可能会较为迷惑]
unset    自然值，如果属性是自动继承相当于inherit, 否则等于initial
		    [对于非继承属性，应用默认值]
		    [对于继承属性，一定是应用继承值，这是与initial的区别]
revert  具体含义取决于origin
	User-agent = unset
	User = Rolls back to the user-agent level
    Author = Rolls back to the user level 
    (the Author origin includes the Override and Animation origins.)
3. all属性利用继承控制值，快速重置所有属性值（实际上不含unicode-bidi & direction）


```



## Selector

```css
selector list
# comma分割，有一个选择器不合法，整个列表都失败
h1,  .cls{}

# Table of selectors
Type selector			  h1 {  }	
Universal selector		   * {  }	
Class selector			  .box {  }	
id selector				  #unique { }	
Attribute selector		   a[title] {  }	
Pseudo-class selectors	 	p:first-child { }	
Pseudo-element selectors 	p::first-line { }	
Descendant combinator	    article p	
Child combinator		    article > p	
Adjacent sibling combinator	h1 + p	
General sibling combinator	h1 ~ p

Types of selectors
1. Type, class & ID selectors
    h1{}  .cls{}  #id{}
1.1 type : aka tag name selector or element selector
1.2全局选择器 [asterrisk *]
可以用universal selector使选择器含义更明确如 
    article *:firstchild = article :firstchild	# article的第一个子元素
	!= article:firstchild	# 表示作为其他元素的第一个子元素的article
1.3 class
1.3.1 Targeting classes on particular elements
	span.cls{ }
1.3.2 Target an element if it has more than one class applied
	.cls1.cls2{ }	# <div class="cls1 cls2">
1.4 id, basicly same as class. (只是id似乎不可重复, 另外一个属性似乎也只能有一个id)
    #id
    h1#heading 
  
2. Attribute selectors
    a[title]{}  
    a[href="#"]{}
2.1 Presence and value
    [attr]	
    [attr=value]	exactly
    [attr~=value]	exactly or contained in a list(space seperated)
    [attr|=value]	exactly or begin with value- (with a hypen)
2.2 sbustring matching 
    [attr^=value]  begin with 
    [attr$=value]  end with
    [attr*=value]  anywhere
2.3 case sensitive #default
    [attr^="a" i]  add `i` before closing braket to force case-insensitive.
    [ s]           add `s` to force sentitive, not very useful though. 

3. Pseudo-class & Pseudo-element
    a:hover{}
    p::firstline{}
3.1 pseudo-class : a `state` of a element, as if you add a class to it.
    p:first-child  #means state of p is first-child of others, but not first-child of p.
3.1.1 simple pseudo-class
	:first-child :last-child :only-child :invalid
3.1.2 user action
	:hover :focus
3.2 pseudo-element: similar, just act as if you create a new element. 
    [though something may be tricky as below]
    p:first-child  #means state of p is first-child of others
    p::firstline   no spaces between p and ::firstline  #means firstline of p
    p span		  has space between p and span
    #[and be careful: some old pseudo-element may use single colon or double both]
3.3 combining 
	article p:first-child::first-line { }
3.4 Generating content with ::before and ::after
    ::before{
        content: 'before notes.';
        display: 'block';
    }

4. Combinators
    article   p {}		# descendant
    article > p {}		# direct child
    article + p {}		# adjacent sibling
    article ~ p {}		# general siblings

```



## Box Model

```css
block-box & inline-box
The full CSS box model applies to block boxes, inline boxes only use some of the behavior.
inline: 不会导致换行；不能设置width/height；垂直方向不会推开其他元素；水平方向会推开；

# display: inline-block   act as block, just dont break onto a newline.  (avoid overlap)

content padding border margin
# standard box model #content-box
width|height padding border
margin is not counted, it just defines the outer space of box.
# alternative box model [box-sizing: border-box]
width|heigth defines the visible box size on the page(that is content+padding+border)
the method to use border-box but not content-box globally:
html {
    box-sizing: border-box;
}
*, *::before, *::after {
    box-sizing: inherit;
}

# margin 上右下左 可以为负
margin会推开其他元素（但如果被推元素设置了固定的width、height，而推开会导致其变化，则不会推开）
margin collapsing: 两个元素的margin会互相折叠，取最大值，如果有负数相减掉
# border 
[border-width border-style border-color]
[border-top border-right border-bottom border-left]
border-top-width...
# padding 上右下左 不能为负
not negative, background applied here.

outer display, inner display
# display: flex 指定子元素按照 flex布局（inner）, 而该元素不变（outer）

```



## BackGround and Border

```css
BackGround
1.background-color
bottom layer

2.background-image
#background-repeat image
	repeat|no-repeat|repeat-x|repeat-y
#background-size image
    cover|contain、  宽|高、  宽
#background-position image 水平 垂直
	background-position: right 1em bottom 10px
    (background-position-x background-position-y)
# gradient
	background: function(..)
linear-gradient(to right, red, orange, yellow, green, blue, indigo, violet);
radial-gradient(red, blue, green)
repeating-linear-gradient(to top left, lightpink, lightpink 5px, white 5px, white 10px)    
repeating-radial-gradient(powderblue, powderblue 8px, white 8px, white 16px)
conic-gradient(lightpink, white, powderblue)
# multiple images
  separating each one with a comma `,`   # from top to bottom.
  background-* properties can also have values comma-separated
#background-attachment
when fixed to sth, it means 他们之间没有相对运动.
when we say scroll or fixed, it is relative to viewport.
	scroll # fixed to the page. 因为跟随页面滚动，相对于页面看起来反而是不滚动的
	fixed  # fixed to the viewport. 视窗是固定的，所以页面滚动时，相对于页面看起来就像是滚动的
	local  # fixed to the element. 元素可能跟随页面滚动，也可能自己滚动

3.shorthand background
background-color 	only after the final comma.
background-size 	only after background-position with a '/' like: center/80%
# example of multi-image and a bk-color [comma sep]
background:   
    linear-gradient(105deg, red 39%, green 96%) center center / 400px 200px no-repeat,
    url(big-star.png) center no-repeat, 
    rebeccapurple;


Border, also see box-model
1.rounded corners
#border-raidus `shorthand`
可以是1|2|3|4 个值， 与边距margin等类似  水平垂直指定不同时用/分割
border-top-left-radius 		horizontal-r vertical-r | r(one value for both)
border-top-right-radius 
border-bottom-right-radius 
border-bottom-left-radius
'sample'
border-raidus: 1em;
border-raidus: 1em 1em/2em;   左上 右下 1em, 右上 左下 1em 2em
border-top-right-raidus: 1em 2em;

```



## Text Direction

```css
writing-mode: horizontal-tb | vertical-rl | vertical-lr

# block dimension:  the direction blocks displayed
# inline dimension: the direction a sentence flows

## Logical properties and values
使用Logical属性让布局属性在不同文字方向的场合兼容（原来的width等属性是和物理屏幕方向关联的）
width -> inline-size
height -> block-size
    top -> block-start
    bottom -> block-end
    left -> inline-start 
    right -> inline-end
margin-top -> margin-block-start
margin-right -> margin-inline-end
margin.. border.. padding..

```



## Content Overflow

```css
内容超出box的尺寸，就会溢出
# 控制溢出行为，默认显示
overflow: visible | hidden | scroll | auto
	scroll : 永远显示scroll bar
	auto: 只在溢出时显示滚动条
overflow是个shorthand <> overflow-x overflow-y

```



## Values & Units

```css
##CSS VALUE aka. data-type. 
<color>

#Numeric
<integer>	整数
<number>	可含小数
<dimension>	尺寸，带单位， 是<length><angle>等的总称类别
<percentage> 
#length - relative and absolute
'absolute'
Unit	Name	Equivalent to
cm	Centimeters	1cm = 96px/2.54
mm	Millimeters	1mm = 1/10th of 1cm
Q	Quarter-millimeters	1Q = 1/40th of 1cm
in	Inches	1in = 2.54cm = 96px
pc	Picas	1pc = 1/6th of 1in
pt	Points	1pt = 1/72th of 1in
px	Pixels	1px = 1/96th of 1in
'relative'
Unit	Relative to
em		Font size of the parent(font-size), or itself(width).
ex		x-height of the element's font. 0.5em
ch		The advance measure (width) of the glyph "0" of the element's font.
rem		Font size of the root element.
lh		Line height of the element.
vw		1% of the viewport's width.
vh		1% of the viewport's height.
vmin	1% of the viewport's smaller dimension.
vmax	1% of the viewport's larger dimension.
#Percentages
related to same property of parent [so width is the percentage of parent's width]
#Numbers
opacity: 0~1  0 transparent 1 opaque #会导致里面子元素也被影响 [区别于颜色的alpha只会影响自己]


#Color
#keywords
lime rebeccapurple
#hexadecimal	代表rgb rgba
    #00ff00   #0f0
    #00ff0088 #0f08
#rgb rgba
	rgb(0, 256, 0, 1)   a:0~1
#hsl hsla
	hsl(188, 97%, 28%)
Hue: 	   The base shade of the color. 0~360 representing the angles round a color wheel.
Saturation: How saturated is the color? 0–100%, 
		where 0 is no color (appear as a shade of grey), and 100% is full color saturation
Lightness: How light or bright is the color? 0–100%,
		where 0 is no light (black) and 100% is full light ( white)

#Images
<image>		
url('img url')
linear-gradient(90deg, red, blue)

#Position
<position>	`background-positon`
[values]: left | top | bottom | right | center  [<length>]
2-value: the first horizontally, the second vertically. 
1-value: the other axis will default to center.
top left
top 1em center

#Strings and Identifiers
red := identifier
"i'm string"

#Functions
width: calc(20% + 100px);

```



## Sizing Items

```css
#intrinsic size

#extrinsic size
'percentage' 
block-level element
width : 50%    1/2th of the space it would normally fill
margins and paddings
margin: 20%    1/5th of the inline size(width in horizontal-tb)
'min and max size'
min-height: 100px
max-width: 100%   如果空间不够会scale down; 但空间大了不会stretch; 很多场合用来控制图像很适合responsive
'viewport unit'
vh vw   0.01
```



## Images, Media and Form Elements

```css
##Replaced Element
# some replaced elements have intrinsic dimension & aspect ratio  (video, img)
可替代元素，内部显示方式会按照自己的规则来显示，不受文档影响
（当然这个元素本身在文档中的位置是受控制的，并且也可以通过特定元素属性来控制其内部内容的布局如object-fit）
<iframe> <video> <embed> <img>				 # typical
<option> <audio> <canvas> <object> <applet>   # only in specific cases
<content added by css> 						# anonymous replaced elements
<input type='image'>   						# as <img>


#image
比空间小不会stretch， 但比空间大会溢出 #通过设置img的max-width可解决
#max-width 自动缩小（避免溢出），但不会拉伸
	<iframe> <video>同样适用
#object-fit 设置图片内容在img的content box里的fit方式（而不是img的parent元素的content box）
fill    默认值，拉伸填满并显示完整内容（拉伸，不保持分辨率），会导致图片扭曲
none    原有尺寸
contain 拉伸自适应（只要求一个维度填满）以显示完整内容（拉伸，保持分辨率），宽荧幕模式letterboxed
cover   拉伸填满（拉伸，保持分辨率），会造成图片被裁剪 
scale-down  宽荧幕模式，类似于contain, 但是只缩小，不放大（此时相当于none）


#forms 
# 文本输入部件可以正常样式化
# 很多部件无法样式化，受操作系统绘制。
# 属性继承，有些浏览器表单部件不会继承font属性，可以手动指定
button, input, select, textarea { 
  font-family : inherit; 
  font-size : 100%; 
} 
# 不同浏览器的forms元素可能使用不同的box-sizing, 同样可以手动维持一致性
button, input, select, textarea {  
  box-sizing: border-box; 
  padding: 0;
  margin: 0; 
}
# 对于文本输入，可以设置只在需要的时候显示滚动条
textarea {
  overflow: auto;
}

# Normalizing Stylesheets
像上面描述的这种规范化的样式表设置，可以在不同的浏览器之间保持很好的一致性
Normalize.css	# just one choice among that.

```



## Styling Tables

```css
Spacing and layout
Some simple typography
# table
table-layout :> fixed | auto	#指定为fixed并且设置width, 就不会再根据cell内容自动调整
width: 100%;
boder-collapse :> collapse | separate	#指定为collapse让单元格之间的边框合并到一起

# td th
padding
border
text-wrap
text-align
font-family
letter-spacing

# graphics and colors
background
color
text-shadow
background-color
background-image
"Zebra striping"
:nth-child(odd|even)

# style caption
caption-side: bottom| top	# 还有其他非标准值，left, right, top-outside, bottom-outside

# Table styling quick tips
1. simple, flexible   # using percentages, more responsive.
2. table-layout: fixed # predictable, easily set column widths by setting on headings (<th>).
3. border-collapse: collapse # neater border
4. <thead>, <tbody>, and <tfoot> # logical chunks, easily layer styles on top of each other 
5. use zebra striping # make alternative rows easier to read.
6. text-align  # line up your <th> and <td> text, neater

```



## Debug CSS

```
1. Take a step back from the problem
2. Do you have valid HTML and CSS?
3. Is the property and value supported by the browser?
4. Is something else overriding your CSS?
5. Make a reduced test case.

```



## Organize Css

```python
# Tips to keep CSS tidy
1. Coding Style Guide
2. Keep Style Consistent
3. Format Readable
4. Comment
5. Logical Sections
  <1>. general styles. body  p  h1,h2..  ul,ol  table  a
  <2>. utilities. for example: a class remove list-style.
  <3>. site-wide. basic page layout, navigation style, the header...
  <4>. specific things. a page, component...
6. Avoid overly-specific selectors
7. Break into multiple smaller css-file


# CSS Methodology
# OOCSS: Object Oriented CSS   (about reuse common css.)
.media			   # reuse
.media .content		# reuse
.comment
.list-item
.comment .img

<div class="media comment">
  <img />
  <div class="content"></div>
</div>

<li class="media list-item">
  <img />
  <div class="content"></div>
</li>

# BEM: Block-Level Element Modifier
The additional classes are similar to OOCSS, despite strict naming conventions.
<form class="form form--theme-xmas form--simple">
  <input class="form__input" type="text" />
  <input
    class="form__submit form__submit--disabled"
    type="submit" />
</form>

# SMACSS: Scalable and Modular Architecture for CSS
# ITCSS
# Atomic CSS


# CSS Tooling
1. Pre-Processor := compile raw to css.
sass
2. Post-Processor := optimize css, like remove spaces & comments.
cssnano
```



# Style Text

## Fundamental Text and Font

```css
#Font Styles
color: red;
font-family: Helvetica, Arial, sans-serif;	# serif, sans-serif, monospace, cursive, fantasy
font-size: 5rem;
font-style: italic;		# normal, italic(a version), oblique(simulated)
font-weight: bold;		# normal, bold...
text-transform: uppercase;	# none, uppercase, lowercase, capitalize, full-width
text-decoration: underline; #shorthand(-line -style -color) none, overline, underline, line-through
text-shadow: 4px 2px 3px red; # (horizontal-offset, vertical-offset, blur-radius, color)

'font shorthand'
-style, -variant, -weight, -stretch, -size+, line-height, -family+  (+ needed.)
font: italic normal bold normal 3em/1.5 Helvetica, sans-serif;

#Text Layout Styles
text-align: right;	# left, right, center, justify
line-height: 1.5;	# <number> <length>
letter-spacing: 2px;	# <length> between character
word-spacing: 2px;  # <length> 

text-indent: 0;
text-oveflow: ellipsis;		# along with "overflow: hidden; white-space: nowrap;"
white-space: nowrap;
...

```



## List

```css
#list-style #shorthand 
	:= no order, -type could be a fallback of -image.
# list-style-type: disc
<custom-ident>, symbols(), <string>
    none, disc, circle, square, '\1F44D'
    decimal, decimal-leading-zero,
    lower-alpha(lower-latin), lower-roman, lower-greek
    simp-chinese-formal, simp-chinese-informal
# list-style-position: outside
	outside | inside
# list-style-image: url('..')

#list-count
<ol start="4" reversed>
  <li value='8'>		# will reset the start to `value 8`
  <li value='5'>
```



## Links

```css
#link-state
:link
:visited	exists in brower history
:hover
:focus		e.g. `tab` to it
:active		when being activated(e.g. clicked on)
color
cursor: pointer | default | text | help | move...	# 鼠标
outline: 1px solid red;		#shorthand 轮廓边框，看上去跟border很相似（outside the border）
	-style: auto|none|dotted|dashed|solid|double|groove|ridge|inset|outset
'could use border-bottom to replace text-decoration'
text-decoration: none;
border-bottom: 1px solid;	# drawn lower(not cut across chars like g, y), more style options


#add image to external links
a[href*="http"]{
  background: url('https://mdn.mozillademos.org/files/12982/external-link-52.png') no-repeat 100% 0;
  background-size: 16px 16px;
  padding-right: 19px;
}


#style links like buttons
e.g. links in the navigator bar
<ul><li><a/></li>..<ul>
un :> no padding, width 100%
li :> set to inline, all links in one line
a :> set to inline-block so could set width
'链接被放在HTML一行，避免之间出现space，影响布局。关于去除空格的更多技巧可参考：'
https://css-tricks.com/fighting-the-space-between-inline-block-elements/

```

## Web Fonts

```css

# Find Fonts
'A free font distributor'
https://www.fontsquirrel.com/
http://www.dafont.com/
https://everythingfonts.com/
'A paid font distributor'
http://www.fonts.com/
http://www.myfonts.com/
> font foundries
https://www.linotype.com/
http://www.monotype.com/
http://www.exljbris.com/
'An online font service'

#steps
# download TTF (True Type Fonts) or OTF (Open Type Fonts).
# webfont generator
@font-face {
    font-family: 'alex_brush';
    src: url('fonts/alexbrush-regular-webfont.woff2') format('woff2'),
         url('fonts/alexbrush-regular-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}
# using in css


# Using an online font service
# subscription-based
'Adobe Fonts'
https://fonts.adobe.com/
'Cloud.typography'
http://www.typography.com/cloud/welcome/
# free
'google fonts'

#steps (no @font-face needed)
<link href="https://fonts.googleapis.com/css2?family=Balsamiq+Sans&display=swap" rel="stylesheet">
font-family: 'Balsamiq Sans', cursive;


# bulletproof @font-face syntax
@font-face {
  font-family: 'ciclefina';
  src: url('fonts/cicle_fina-webfont.eot');
  src: url('fonts/cicle_fina-webfont.eot?#iefix') format('embedded-opentype'),
         url('fonts/cicle_fina-webfont.woff2') format('woff2'),
         url('fonts/cicle_fina-webfont.woff') format('woff'),
         url('fonts/cicle_fina-webfont.ttf') format('truetype'),
         url('fonts/cicle_fina-webfont.svg#ciclefina') format('svg');
  font-weight: normal;
  font-style: normal;
}

font-family:= name to use
url:= comma separated, normally newer type first, except that eot placed first(to fix bugs of old-ie)
font-weight/font-style

font-variant
font-stretch
unicode-range


# Other
# Variable fonts
fonts that allow many different variations of a typeface to be incorporated into a single file, rather than having a separate font file for every width, weight, or style.

```



# CSS Layout

## Introduction

```css
#Normal flow:
how the browser lays out HTML pages by default when you do nothing to control page layout.
display: inline | block | inline-block

#Flex & Grid
display: flex grid

#Float
float: none left right inherit

#Positioning 
{
    positon: relative;
    top = 20px;
    left = 20px;
}
static		:= default
relative	:= offset an item from the position in normal flow it would have by default
absolute	:= 'move out of normal flow'(important). 
			relative to the edge of <html> element (or its nearest positioned ancestor element).
fixed		:= 'move out of normal flow', 
			realtive to viewport
sticky		:= like 'static', but act like 'fixed' when hit a defined offset from viewport

#table-layout
form {
    display: table;
}
form > div {
    display: table-row;
}
form label, form input{
	display: table-cell;
}
form > p {
    display: table-caption;
}

#Multi-column layout := lay out content in columns, similar to how text flows in a newspaper.
column-count: 3;  # 3 cols
or
column-width: 200px;  # as many as could, with each col having 200px-width.

```



## FlexBox

```css
# flexbox
one-dimensional layout method, flex to fill additional space and shrink to fit into smaller space.


```

