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

