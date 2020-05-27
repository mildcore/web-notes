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
// 注意js内部函数调用, `-无-↓-法-`访问外部函数变量。但可以访问全局变量。
// 更正: 应该是也可以访问外部函数变量的，经试验可以实现闭包。

//箭头函数 arrow function.
// (x) => x 是 (x)=> {return x;}  的有效简写;
x => x*x;		//定义一个匿名函数，返回平方 
(x, y) => x+y;	 //定义一个匿名求和函数


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


# Event Concepts
// event object. event/evt/e
function bgChange(e){
    e.target.style.backgroundColor = 'rgb(0, 0, 123)';
}
btn.addEventListener('click', bgChange);

// 阻止默认行为，如表单提交。 Preventing default behavior
e.preventDefault();

// Event bubbling and capture (two different phase)
capturing phase: from outer to inner, check and run eventhandler.
bubbling phase: opposite.
// by default, all event-handlers are registered for bubbling phase.
// if both types exist. capturing phase run first ->  then bubbling 

// 阻止事件传播
e.stopPropagation();

```



# JavaScript Objects

## Basic

```javascript
// 对象字面量 object literal
var book = {name: 'Python CookBook', author: 'Michael Jim', price: 100}

# 对象访问属性两种方法
book.name 		//dot notation
book['name']	//Bracket notation (objects are sometimes called associative arrays[关联数组])
# 使用括号表示法，能使用变量做变量名
var attrname = 'at1';
var attrvalue = 'value';
book[attrname] = attrvalue;		
book.at1 = 'value';		// 等同于这个效果

# this, js使用this关键字引用当前对象
this.name		

// 对象构造函数 construct function
function Person(name) {
  this.name = name;
  this.greeting = function() {
    alert('Hi! I\'m ' + this.name + '.');
  };
}
var jack = new Person('jack');

# Other ways to create object instances
// Object() constructor. 直接new一个空对象
var person1 = new Object();   			// 构造一个空对象。还可以直接传入一个字典（object literal）来构造Object对象， 有点奇怪哈。
// Object.create() 基于现有对象构建一个对象
var person2 = Object.create(person1)	// 有点像是复制

```



## Prototype

```javascript
javascript: 基于原型的语言 prototype-based language.
每个对象拥有一个`prototype对象`，从原型对象继承方法和属性。 // Object.getPrototypeOf(obj) 或者 obj.__proto__
原型对象也可以有自己的原型对象，从而形成原型链。Prototype Chain.
属性和方法定义在对象的构造器函数的`prototype属性`上

# 关于对象的原型对象和构造器的原型属性
// 前者是实例的属性，后者是构造器的属性
`Object.getPrototypeOf(new Foobar())` refers to the same object as `Foobar.prototype`

function doSomething(){}
doSomething.prototype.foo = "bar";      // 属性定义在构造器函数的原型属性上， 只有定义在构造器原型属性上的方法属性才被继承
console.log( doSomething.prototype );

# prototype属性： 继承成员定义的地方
// Object.create()
ins2 = Object.create(ins1);		// 实际上把ins1作为了ins2的prototype对象
// constructor
ins2.constructor;		// 可以获取实例的构造器函数

// 修改原型后会动态更新，已生成实例也会更新到最新的方法和属性。
// 原因应该在于js的继承并非是复制方法和属性，而是通过proto-chain动态寻找。

// js的对象定义模式
// 在构造器（函数体）中定义属性、在构造器的prototype属性上定义方法
function Test(a,b,c,d) {
  this.a = 1;						  // 定义属性
};
Test.prototype.x = function () { ... }	// 定义方法

```



## Inheritance

```javascript
// 父类
function A(name){
    this.name = name;
}
A.prototype.say = function(){return 'i am ' + this.name};
// 子类
function B(name, sex){
    A.call(this, name);
    this.sex = sex;
}
B.prototype = Object.create(A.prototype);	// 要从prototype继承方法，必须手动指定构造器的原型属性，从父类的构造器原型属性衍生过来；
B.prototype.constructor = B;			   // 前面代码改变了constructor的值，所以必须重新指定prototype属性的constructor值。

// ew..麻烦的js继承...果然不应该叫做面向对象语言
// 不管怎样，这并不是js继承实现的唯一方式。
较新的js提供了class定义。
另外一些js库也提供了一些方法来实现继承, 比如coffeescript.'http://coffeescript.org/#classes'

```



## JSON

```javascript
JavaScript Object Notation : 使用类javascript对象数据格式来表示结构化数据的一个基于文本的数据格式.
// parse, stringify
JSON.parse(json_txt)
JSON.stringify(json_obj)

```



# JavaScript Asynchronous

## Introducing Asynchronous JavaScript 

```javascript
1.Async Callbacks
function loadAsset(url, type, callback) {
  //..
  xhr.onload = function() { callback(xhr.response); };
}

2.Promises
// 结构： .then(func).then(func).catch(func)
fetch('products.json').then(function(response) {
  return response.json();
}).then(function(json) {
  //..
}).catch(function(err) {
  console.log('Fetch problem: ' + err.message);
});

// Promise会进入事件队列；主线程执行完毕，再依次执行事件队列；

// 比较：Promise本质上是一个返回的对象，可以直接将回调附加到对象上。其优势：
1. 使用then连接多个回调，避免回调地狱(末日金字塔)。callback hell (pyramid of doom)
2. Promise严格按照放在事件队列里的顺序执行。
3. 错误处理更容易，直接放在最后的.catch()



```



## Cooperative Async JS: Timeouts and intervals

```javascript
// 这一节的3个方法，依然是在主线程下执行，所以注意不要滥用
setTimeout
setInterval
requestAnimationFrame

// Timeout, 执行一次
// 函数， 至少等待时间（ms）, 传给函数的参数值（0个或多个）
timeoutRef = setTimeout(func, 2000); // 返回值可以用来指代这个timeout, 实际等待时间是大于2000不确定的
clearTimeout(timeoutRef);			// 可以利用返回值清除某个timeout

// Interval, 周期执行
const myInterval = setInterval(myFunction, 2000);
clearInterval(myInterval);

# 注意点
1. 递归的timeout可以代替interval.
// 区别在于timeout的时间是上一次结束到下一次开始, interval时间包含了执行时间。
// 所以interval可能会发生执行时间超出等待时间的问题。
toRef = setTimeout(func, 2000);
function func(){
    //..
    setTimeout(func, 2000)
}
2. 立即结束
setTimeout(func, 0)		// 主线程执行完成后，立即执行该任务。

// requestAnimationFrame
// 专门的循环函数，旨在在浏览器高效运行动画。是对interval的改进版（帧率不按设备优化, 有时丢帧; 页面未激活、组件滚出屏幕仍在运行等）。
function draw(){
    //draws..
    requestAnimationFrame(draw);
};
draw();
// requestAnimationFrame的回调函数可以接收一个时间戳timestamp
function draw(timestamp){..}
// ie10以上就已经支持requestAnimationFrame
// 取消,类似于前两个
cancelAnimationFrame(rafRef);

```



## Promise

```javascript
一个promise只能成功或失败一次。并且一旦操作完成，它就无法从成功切换到失败，反之亦然。
如果promise成功或失败，并且您稍后添加成功/失败回调，依然将调用正确的回调，即使事件发生在较早的时间。

基本语法
.then(resp => {}).catch(e => {}).finally(() => {});

概念
1. 一个promise被创建后处于'pending'状态。
2. promise返回时，称作已被'resolved'. 成功resolved，叫'fulfilled'; 失败叫'rejected';

# 多个promise fulfilled.
// 利用Promise.all(), then会传递一个结果的数组
// 必须全部成功实现，有一个失败，就会rejected. 
// 如果单个promise内部实现了catch, 则all不会表现为失败, 而是得到一个undefine值.
Promise.all([p1, p2, p3]).then(values => {})

# 构造自定义promises
1. 使用Promise()构造函数
// 需要创建一个函数传入构造器方法，这个函数需要接收两个函数参数，resolve和reject, 用来完成或拒绝；
new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('ok now.')
    }, 2000)
}).then( val => alert(val));

2. reject
// 一个function内部实现了异步， 这个function便也成了异步
function myPromise(interval){
    return new Promise((resolve, reject) => {
        if (interval < 0 || typeof interval !== 'number'){
            reject('interval is negative or not a number.')
        } else {
            setTimeout(() => resolve('ok now.'), 2000)
        }
    });
}
```



## async & await

```javascript
// async把一个函数变成async function
async function hello(){return 'hello'}; 
hello();	//函数调用会返回一个promise, 而不是直接返回结果
// await只能用在async function里，用来等待一个async promise-based function的调用
async function hello() {
  return greeting = await Promise.resolve("Hello");		// return和赋值可以同时使用，应用的是同一个值，js还挺有特色
};
hello().then(alert);

// .then 转化为 async/await
fetch('url').then(resp => resp.blob()).then(blob => URL.createObjectURL(blob));
async function geturl(){
    let resp = await fetch('url');
    let blob = await resp.blob();			// 只有promise才需要await, blob也是个async promise-based function
    
    url = URL.createObjectURL(blob);		// 这个不是async promise-based function
    return url;
}
geturl().catch(e => console.log(e.message));

// 错误处理
1. async函数里可以直接使用 try .. catch ..
2. 函数调用后，对promise使用.catch, 就像以前一样（会同时捕捉异步函数和promise链[.then]里的错误）

// 等待Promise的方法
let values = await Promise.all([coffee, tea, description]);

// 关于await的执行时间
// 直接在调用上await, 会等待前一个执行完，才执行下一个。总时间2s
await MyPromise(1000);
await MyPromise(1000);
// 先把调用结果存到变量，最后再一系列await，是同时执行的。总时间1s
`值得注意，不过js这个语言特色很奇怪的样子. 对几个存到变量里的promise连续await, 是同时执行, 不会等待一个执行完才执行下一个`
const p1 = MyPromise(1000);
const p2 = MyPromise(1000);
await p1;
await p2;

# 类方法async
// 定义类Person
class Person {
  constructor(first, last, age) {
    this.name = {
      first,
      last
    };
    this.age = age;
  }

  async greeting() {
    return await Promise.resolve(`Hi! I'm ${this.name.first}`);
  };
    
  farewell() {
    console.log(`${this.name.first} has left the building. Bye for now!`);
  };
}
// 调用async方法
new Person('jim', 'bob', 28).greeting().then(console.log);

# 浏览器支持
可以利用'BabelJS'库实现js代码的回退，以支持老式浏览器的正常使用。

```

