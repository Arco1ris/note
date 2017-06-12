#### async

用来处理异步函数，设计之初是用在 Node.js 上，但是同样可以直接用在浏览器端。

Multiple callbacks

确保当每次调用 callback 的时候都 return ，否则你将导致多重回调嵌套以及不可预测的行为。



Control Flow

**series(task,[callback])**

运行在 series 中 task 数组内的函数，每个函数将在前一个函数完成时执行。如果在 series 中的任何函数传递 error 在回调函数中，将不再有函数执行，然后 callback 马上被调用，带上 error 作为参数。此外，callback 接受一个数组的结果当 task 被执行完。

同样可以用一个 Object 替代 Array，每个属性将被作为一个函数去运行，然后这些结果（同样是Object的结构）将被放在最后的 callback 里。

```javascript
async.series([
  function(callback){
    // do some stuff
    callback(null,'one');
  },
  function(callback){
    //do some more stuff
    callback(null,'two');
  }
],
  // optional callbak
   function(err,results){
    //result is now equal to ['one','two']
  })

// 不用 array 用 object
async.series({
  one:function(callback){
    setTimeout(function(){
      console.log('ONE');
      callback(null,1);
    },200);
  },
  two:function(callback){
    setTimeout(function(){
      console.log('TWO');
      callback(null,2);
    },100);
  },
  function(err,results){
    // results is now equal to :{one:1,two:2}
  }
})
// ONE TWO
// 即使 one 设置的 setTimeout 时间比 two 多，但是仍然要先执行 one,结束了以后再执行 two。 


```

函数之间没有必然联系，只是产生的结果被放在 array 里或者 object 里，如果前一个函数报错了，那下面的函数将不被执行。每个函数执行之前都要等待前一个函数执行完毕，上面函数的执行顺序一定是先执行 one，再执行 two，如果 two 的 callback 为 `callback('ERROR',2);` 会输出 error 的值 'ERROR' 和 one two 的值，因为 two 传递的 callback 不会影响到之前已经执行了的函数的值，但是此时如果 two 后面还有函数要执行，则不会被输出。如果 one 的 callback 传递了 error ，则会导致只输出 error 和 one，不会输出 two。



**paralle(task,[callback])**

同样是运行一个 task array（也可替换为object），但是不需要等待前一个 function 执行完成，如果任何函数产生error到它的callback，这个主callback将会立即被调用（带上 error 参数），一旦 task 被执行完毕，结果会通过最后的callback（以array的形式）。

```javascript
async.parallel([
    function(callback){
        setTimeout(function(){
          console.log('ONE');
            callback(null, 'one');
        }, 200);
    },
    function(callback){
        setTimeout(function(){
          console.log('TWO');
            callback(null, 'two');
        }, 100);
    }
],
// optional callback
function(err, results){
    // the results array will equal ['one','two'] even though
    // the second function had a shorter timeout.
});
// TWO ONE
```

上面的函数是先执行下面这个函数，再执行上面这个，因为 setTimeout 下面这个时间短。

**waterfall(task,[callback])**

运行 task array ，每个都要将它们的结果传给 array 中的下一个函数，如果任何函数产生了 error，下一个函数将不被执行。主 callback 将立即执行（带上 error）。

```javascript
// 直接这样好像不能传递参数给第一个 function
async.waterfall([
    function(callback) {
        callback(null, 'one', 'two');
    },
    function(arg1, arg2, callback) {
      // arg1 now equals 'one' and arg2 now equals 'two'
        callback(null, 'three');
    },
    function(arg1, callback) {
        // arg1 now equals 'three'
        callback(null, 'done');
    }
], function (err, result) {
    // result now equals 'done'
});


// 或者 

async.waterfall([
  // 给第一个函数带入参数
    async.apply(myFirstFunction, 'zero'),
    mySecondFunction,
    myLastFunction,
], function (err, result) {
    // result now equals 'done'
});
function myFirstFunction(callback) {
  callback(null, 'one', 'two');
}
function mySecondFunction(arg1, arg2, callback) {
  // arg1 now equals 'one' and arg2 now equals 'two'
  callback(null, 'three');
}
function myLastFunction(arg1, callback) {
  // arg1 now equals 'three'
  callback(null, 'done');
}
```

如果 myFirstFunction 传递给下一个 function 的 callback 为 `callback('error',arg1)`，则不会继续往下执行下面的函数，会直接调用最后的那个 callback `function(err,result){}`，并且如果 myFirstFunction 原本传递给下一个 function 即 mySecondFunction 的参数，也被传递到了最后这个函数内。



**compose(fn1,fn2..)**

通过组合嵌套多个函数创建出来一个函数并执行，每个函数都通过调用前一个函数传递过来的值。 组合函数 f(), g(), h(), 将会产生 f(g(h()))。



```javascript
function add1(n, callback) {
    setTimeout(function () {
        callback(null, n + 1);
    }, 20);
}

function mul3(n, callback) {
    setTimeout(function () {
        callback(null, n * 3);
    }, 10);
}
// 被包裹的函数放后面，所以add1放在mul3后面，add1() 在最里面，add1 的值传给 mul3 
var add1mul3 = async.compose(mul3, add1);

add1mul3(4, function (err, result) {
   // result now equals 15
});
```

任何一个函数报错，报错后的值传到 callback，只会影响后面需要执行的函数，不会影响之前的值，（不过这个应该只有一个参数，不能有多个参数。）