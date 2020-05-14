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

