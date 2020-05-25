# JavaScript First Step



## JavaScript Basic

```javascript
javascript: 动态脚本语言，用来创建动态更新的内容。控制多媒体，制作图像动画等等。
a simple demo:
const para = document.querySelector('p');

para.addEventListener('click', updateName);

function updateName() {
  let name = prompt('输入一个新的名字：');
  para.textContent = '玩家1：' + name;
}

# API
## 浏览器API
	DOM API
    GeoLocation API
    Canvas and WebGL 	: 创建生动的2D和3D图像
    HttpMediaElement and WebRTC(Real-Time Communications)	: 音视频API  
## 第三方API
	谷歌地图API, Twitter API...
    
# JS加载
## 内部js
<script>
    ...
</script>
## 外部js
<script src="script.js" defer></script>
## inline js
不建议，污染html且低效
## js加载策略
// 内部js
document.addEventListener("DOMContentLoaded", function() {...});
// 外部策略
async // 脚本下载完立刻执行，不保证顺序
defer // 脚本和content下载完后执行，且保证执行顺序

# JS BASIC
// js区分大小写

//comments
// ..
/* .. */

// 变量 let (var)
// var hoisting, 变量提升； 即声明可以在使用之后，并且可以多次声明；let修复了这个问题，let不可以这么做。
let a = 1;

// 常量 const
const para = document.querySelector('p');
    
// 函数 function
function funcname(){};
    
//operator
+ - * /
+=
++ --
===	//相等判断3个=
!== //不等判断2个=

//conditon
if () {}
else if () {}
else {}
    
// loop
for (let i = 1; i < 21; i++) { console.log(i); }
    
//event
para.addEventListener('click', updateName);    
                                                          
# 打印文本到控制台
console.log()
    
```



## Variable

```javascript
变量类型
数据类型
//Number
let myAge = 17;
//String
//Boolean
true false
//Array
[1, 2, 3]
//object
let dog = { name : 'Spot', breed : 'Dalmatian' };

js: 动态类型语言
typeof myAge;

```



## Number

````javascript
对于数字只有一个数据类型
Number: 包含integer, decimals.

// 算术运算符
// + - * / % **

//自增++ 自减--
a++
++a

//赋值运算符
= += *= ...

//比较运算符
===  !==  >  <  >=  ...
== != // 只测试值是否相等，不看数据类型； 前面的===则要严格相同；
````



## Strings

```javascript
let mystr = 12 + 'b'; //合法

// 字符串转数字
let myNum = Number("12");
// 数字转字符串
let myNum = 123;
let myString = myNum.toString();

# 常见字符串操作方法
//长度
myStr.length;
//获取元素
myStr[0];
//子串检索
myStr.indexOf("zi");	// 没有找到返回-1
//切片
myStr.slice(0,2);		// 第二个参数，可选，没有时直到结束；
//大小写
toLowerCase() toUpperCase()
//替换
myStr = myStr.replace('src', 'dst');	// replace不会更改被操作字符串，而是直接返回一个新串

```



## Arrays

```javascript
let li = [1, 'a', ['hello', 'js']];
//访问元素
li[2][1]
//长度
li.length;
//增加元素， 删除元素
li.push('Bradford', 'Brighton'); 	//可一次push多个，返回值最新长度
li.pop();						  //删除返回最后一个元素
unshift('st');					   //作用于开头
shift();						  //作用于开头


# 字符串和数组的转换
//字符串到数组
let myStr = 'a,b,c';
myStr.split(',');
//数组到字符串
let strs = ['a', 'b', 'c'];
strs.join(',');

```



# JavaScript Building Blocks

## Condition

```javascript
// if-condition
// false : [false, undefined, null, 0, NaN, '']
// true: 其他
if () {}
else if () {}
else {}

// &&  ||  !

// switch
switch(a){
    case val1:
        ..
        break;
    default:
    	..        
}

// 3元运算符
（cond） ? ex1 : ex2;

```

## Loop

```javascript
// for loop
for (var i = 0; i < 100; i++) {
    ..
}

// break, continue

// while， do..while
while (cond){}
do {} while (cond)

```

## Function

```javascript
// define a function
function funcname(){}

// anonymous function
let button = document.querySelector('button');
button.onclick = function() {}

// scope作用域
// 注意js内部函数调用, 无法访问外部函数变量。但可以访问全局变量。

```

## Events

```javascript
// 1.Event handler properties
btn.onclick = bgChange;		// bgChange is a function.
btn.onclick = fun2();		// 会覆盖前面的，只能有一个注册的事件处理

// 2.addEventListener() and removeEventListener()
// 可以注册多个事件处理
btn.addEventListener('click', bgChange);
btn.addEventListener('click', functionB);	// 不会覆盖，两个都有效
btn.removeEventListener('click', bgChange);  // 移除一个事件处理器

// 3.Inline event handlers — don't use these
// event handler HTML attributes 
<button onclick="bgChange()">Press me</button>


```

