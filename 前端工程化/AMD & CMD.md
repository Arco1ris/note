JS 的模块化

**一、原始写法**

只要把不同的函数（以及记录状态的变量）简单地放在一起，就算是一个模块

``` javascript
function m1(){
  //...
}

function m2(){
  //...
}
```

上面的函数 m1() 和 m2()，组成一个模块，使用的时候直接调用就行了。

缺点：**“污染”了全局变量**，无法保证不与其他模块发生变量名冲突，而且模块成员之间看不出直接关系。

**二、对象写法**

为了解决上面的缺点，可以把模块写成一个对象，所有模块的成员都在对象里面。

``` javascript
var module1 = new Object({
  _count : 0,
  m1 : function (){
  		// ...
	},
  m2 : function (){
  		//...
	}
})
// 使用的时候
module1.m1()
```

缺点：这样的写法会暴露所有的模块成员，内部状态可以被改写。

**三、立即执行函数写法**

使用立即执行函数，可以达到不暴露私有成员的目的。

``` javascript
var module1 = (function(){
 var _count = 0;
 var m1 = function(){
  //...
 };
  var m2 = function(){
  //...
 };
  return {
    m1 : m1,
    m2 : m2
 };
})();
```

module1 就是 JavaScript 模块的基本写法，下面进行进一步加工。

**四、放大模式**

如果一个模块很大，必须分成几个部分，或者一个模块需要继承另一个模块，这时就有必要采用“放大模式”。

``` javascript
// 为 module1 模块添加了一个新方法 M3(),然后返回新的 module1 模块
var module1 = (function (mod) {
  mod.m3 = function () {
  //...
 };
  return mod;
})(module1);
```

在浏览器环境中，模块的各个部分通常都是从网上获取的，无法知道哪个部分会先加载。

缺点：第一个执行的部分有可能加载一个不存在的空对象

**五、宽放大模式**

``` javascript
var module1 = (function (mod){
  //...
  return mod;
})(window.module1 || {});
```

成功解决“放大模式“的问题，”宽放大模式“就是”立即执行函数“的参数可以是空对象。

**六、输入全局变量**

独立性是模块的重要特点，模块内部最好不与程序的其它部分直接交互。

为了在模块内部调用全局变量，必须**显示地将其它变量输入模块**。

``` javascript
var module1 = (function($,YAHOO) {
  // ...
})(jQuery,YAHOO);
```

上面的模块需要使用 jQuery 库和 YUI 库，就是把这两个库（其实是两个模块）当做参数输入 module1。这样做除了保证模块的独立性，还使得模块之间的依赖关系变得明显。

**七、模块的规范**

模块之所以重要是因为有了模块，我们就可以更方便地使用别人的代码，想要什么功能，就加载什么模块。**但是有一个前提，就是大家必须以同样的方式编写模块。**

目前同性的 Javascript 模块的规范共有两种 CommonJS  (CMD) 和 AMD 

**八、CommonJS**

服务端必须要有模块与操作系统和其它应用程序互动，否则根本没法编程。

node.js 的模块系统是参照 CommonJS 规范实现的。

**在 CommonJS 中，有一个全局性方法 require() 用于加载模块。**

假设有一个数学模块 math.js ，就可以像下面这样加载，然后就可以调用模块提供的方法：

``` javascript
var math = require('math');
math.add(2,3);//5
```

**九、浏览器环境**

有了服务器端模块以后，大家就想要客户端模块。而且最好二者能够兼容，一个模块不需要修改就能在服务器和浏览器都能运行。

但是 CommonJS 规范不适用于浏览器环境，刚才的代码在 `requir('math')`之后运行，因此必须等待 math.js 加载完成，如果加载时间很长，整个应用就会停在那里等。这对服务器端不是问题，但是对于浏览器来说会使浏览器处于“假死”状态。**因此浏览器的模块不能采用同步加载，只能采用异步加载。这就是 AMD 规范诞生的背景。**

十、AMD （Asynchronous Module Definition）

它采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会执行。

AMD 也采用 require() 语句加载模块，但是不同于 CommonJS，它要求两个参数：

``` javascript
require([module],callback);
// 第一个参数是一个数组，里面放置要加载的模块，第二个参数的回调函数，
require(['math'],function(math){
  math.add(2,3);
})
math.add() 与 math 模块加载是不同步的，浏览器不会发生假死
```

目前主要有两个 Javascript 库实现了 AMD 规范，require.js 和 curl.js 

### Require.js

一、为什么要用 require.js

以前，所有的Javascript代码都写在一个文件里，只要加载一个文件就够了，后来代码越来越多，必须分成多个文件，依次加载。

缺点：1. 加载的时候浏览器会停止渲染，加载的文件越多，网页失去响应的时间就会越长。2. 由于 js 文件之间存在依赖关系，因此必须严格保证加载顺序。依赖性最大的模块一定要放在最后加载，当依赖关系很复杂的时候，代码的编写和维护都会变得困难。

Require.js 就是解决了以上两个缺点

二、require.js 的加载

1. 先去官网下载最新版 require.js
   
2. 下载后将它放在 js 的子目录下面 `<script src="js/require.js"></script>`
   
   加载这个文件也可能使网页失去响应，解决办法：1.将其放在网页底部加载；2.写成这样`<script src="js/require.js" defer async="true" ></script>`
   
   async 属性表明这个文件需要异步加载，避免网页失去响应，IE不支持这个属性，只支持defer,所以把defer也写上。
   
3. 加载 require.js 以后，需要加载我们自己的代码 main.js ，也放在 js 目录下面，那么只需要写成下面这样就可以了：
   
   `<script src="js/require.js" data-main="js/main"></script>`
   
   data-main 属性的作用是，指定网页程序的主模块，在上例中，就是js目录下面的main.js，这个文件会第一个被 require.js 加载，由于 require.js 默认的文件后缀名是js，所以可以把main.js简写成main

三、主模块的写法

main.js 是主模块，意思是整个网页的入口代码，一般主模块依赖于其它模块，这时就要使用 AMD 规范定义的 require() 函数。

``` javascript
//main.js
require(['moduleA','moduleB','moduleC'],function(moduleA,moduleB,moduleC){
  //some code here
});
// 实际例子
require(['jquery','underscore','backbone'],function($,_,Backbone){
  //some code here 
})
```

四、模块的加载

上节中，主模块依赖的模块是['jquery','underscore','backbone']。默认情况下，require.js假设和这三个模块与main.js 在同一个目录。

使用require.config() 方法，我们可以对模块的加载行为警醒自定义。require.config()就写在主模块 main.js 的头部。参数就是一个对象，这个对象的 paths 属性指定各个模块的加载路径。

``` javascript
//默认路径与main.js 在同一个目录下。
require.confgi({
  paths:{
    "jquery":"jquery.min",
    "underscore":"underscore.min",
    "backbone":"backbone.min"
	}
})
// 默认路径不同
require.config({
  paths:{
    "jquery":"lib/jquery.min",
    "underscore":"lib/underscore.min",
    "backbone":"lib/backbone.min"
}
})
//直接改变基目录
require.confgi({  
  baseUrl:"js/lib",
  paths:{    
    "jquery":"jquery.min",    
    "underscore":"underscore.min",    
    "backbone":"backbone.min"    
  }})
//如果某个模块在另一台主机上，也可以直接指定它的网址
require.config({
  paths:{
    "jquery":
    "https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min"
	}
})
```

require.js 要求，每个模块是一个单独的js 文件，这样的话，加载多个模块就会发出多次 HTTP 请求，会影响网页的加载速度。因此 require.js 提供了一个优化工具，当模块部署完毕以后，可以用这个工具将多个模块合并在一个文件中，减少HTTP 请求数。

五、AMD 模块的写法

require.js 加载的模块必须按照AMD的规定来写。

具体来说，就是模块必须采用特定的 define() 函数来定义。如果一个模块不依赖其它模块，那么可以直接定义在 define() 函数中。

``` javascript
//math.js
defien(function(){
  var add = function (x,y){
  		return x+y;
	};
  return {
    	add:add
	};
});
如果这个模块还依赖其它模块，那么define()函数的第一个参数必须是一个数组，指明模块的依赖性,当require() 函数加载这个模块时，就会先加载 myLib.js 文件
define(['myLib',function(myLib){
  function foo(){
  myLib.doSomething();
}
  return {
    foo : foo
}
}])
```

加载方法如下：

``` javascript
//main.js
require(['math'],function(math){
  alert(math.add(1,1));
})
```

六、加载非规范的模块

由于有的库符合AMD规范，有的不符合。require.js同样能够加载不规范的模块。

不过要先用 require.config() 方法去定义它的一些特性。

比如 underscore和backbone这两个库，都没有采用AMD规范编写。如果要加载它们的话，必须先定义它们的特征。

``` javascript
// shim 属性专门用来配置不兼容的模块。
require.config({
  shim:{
    'underscore':{
      exports:'_'  //输出的变量名
	},
    'backbone':{
      deps:['underscore','jquery'], // 该模块的依赖性
      exports:'Backbone'
	}，
    'jquery.scroll':{
    deps:['jquery'],
    exports:'jQuery.fn.scroll'
}
	}
})
```

