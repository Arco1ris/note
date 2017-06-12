Async/Await 是 ES7中的特性，它允许你写异步的时候看起来跟同步一样。

它允许你通过 async 作为前缀去申明，通过 await 关键字暂停你的 function，等待 promise 去处理并且返回值。Generator 用 yield 的时候，是返回一个值，而用 await 是返回一个promise，如果 resolve 了，那就会返回一个值。

use the `await` keyword to pause your function in a non-blocking way while you make your asynchronous function call that returns a promise. 

当它返回值（from the promise），这个函数将会继续执行。

``` javascript
function getUserFromDb(){
  throw new Error('No users in db');
}
async function testTryCatch(){
  let response;
  try{
  response = wait getUserFromDb();
} catch (err){
  console.log(err.message);
  response = {user:'default'};
}
  const { user } =response;
  return user;
}
testTryCatch();
```

如果用 promise 

``` javascript
db.get('docid').catch(function(err){
  if(err.status = 404){
  return {};
}
  throw err;
}).then(function(doc){
  console.log(doc);
})
```

如果用 Async/Await

``` javascript
let doc;
try {
  doc = await db.get('docid');
} catch (err){
  if(err.status === 404){
  		doc = {};
	} else {
  		throw err;
	}
}
console.log(doc);
```

Generator 是由 Iterator 和 XX 组成的。

Generator 主要是希望通过把异步的函数写得更同步一样，但是又想要有异步的效果。因为不会阻塞。

两个作用分别是拉数据和推数据。？



Generator 返回一个遍历器对象（Iterator  Object），yield 会将自己后面表达式的值，作为返回的对象的 value 属性值。（yield value）

``` javascript
function* g(){
  try{
  	var a = yield foo('a');
    var b = yield foo('b');
    var c = yield foo('c');
	} catch(e){
  	console.log(e);
	}
  console.log(a,b,c);
}
```

一旦 Generator 执行过程中抛出错误，就不会再执行下去了。

Async 返回一个promise 对象

Async function*  返回一个