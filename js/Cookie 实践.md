如何使用 cookie 和 session 确保用户登录？HTTP 保持状态？

1. 什么是 cookie? 

   cookie 是 web 开发中用于存储信息的对象。js 一般存在于客户端，所以 js 可以直接操作 cookie 进行信息的存储和读取。

   一般写法：

   ```javascript
   document.cookie = "userName = admin";
   ```

   ​

2. 什么是 session? 

   服务端为每一个 session 维护一份会话信息数据，而客户端和服务端依靠一个全局唯一的标识来访问会话信息数据。

   创建 session 可以概括为三个步骤：

   1. 生成全局唯一标识符 sessionID
   2. 开辟数据存储空间。一般会在内存中创建相应的数据结构。
   3. 将 sessionID 发送给客户端

3. 如何将 sessionID 发送给客户端：

   1. Cookie:服务端只要设置 Set-cookie 头就可以将 session 的标识符传送到客户端。而客户端此后的每次请求都会带上这个标识符。由于 cookie 可以设置失效时间，所以一般包含 session 信息的 cookie 会设置失效时间为0，即浏览器进程有效时间。

   2. URL 重写：

      ​

4. 它们的区别在哪里?

   cookie 是存在客户端，session 是存在服务端

5. 如何使用 cookie?

6. 为什么通过 session 能得知用户的状态？需要依赖哪些数据？

7. 是否每个页面都需要查看 session 才能判断该做什么？

8. 如何用 session 记录用户登陆？

   当用户登陆，将会记录 session ，当用户登出，将会销毁session

9. express-session 是必须的吗？如何使用。

   express-session 是 基于 express 框架专门用于处理session的中间件，session 的认证机制离不开cookie，所以同时需要使用 cookieParse 中间件。

   ```javascript
   var express = require('express');
   var session = require('express-session');
   var cookieParser = require('cookie-parser');

   var app = express();
   app.use(cookieParse());
   app.use(session({
     secret:'123456',
     name:'testapp',// 这里的name值就是cookie的name,默认cookie的names是：connet.sid
     cookie:{maxAge:80000},// 设置 maxAge 是 80000ms，即80s后session 和相应的 cookie 失效过期
   }))

   app.get('/awesome',function(req,res){
     if(req.session.lastPage) {
       console.log('Last page was:',req.session.lastPage);
     }
     req.session.lastPage = '/awesome';//每次访问时，session 对象的 lastPage 会自动的保存或更新内存中的 session 中去。
     // res.send();
   })
   ```

10. 为什么永远无法读取到 cookie 的值？？？

   有的说是 https 的原因，有的说是 cookieParser 的原因，有人说是 resave:false, 或者 secure: false, 的原因，明显看到能设置 有set-cookie 了，但是 cookie 值无法存储.

1. favicon 是什么？有什么用

    favourite icon 收藏夹图标，网页对应的图标，被收藏夹收藏时显示的图标

   ​


SessionID 可以存储在 内存，硬盘，数据库，Redis，文件，第三方API 中（比如邮件，微博等等等） 

sessionID 是一个随机数

+ 内存：全局变量，sessionID 的随机数为 key，用户名为 value，每次请求查看对应的随机数是否有value，有则说明已经登录，如果退出登录时，则清空全局变量的内容


SessionID 定时销毁：settimeout，通过时间戳计算，redis 的TTL（time to live）



session 存储方式1：内存

```javascript
// 创建一个随机数
function RandomString() {
    var data = [0,1,2,3,4,5,6,7,8,9,'a','b','c','d','e','f','g','h','i','j'];
    var newData = '';
    for(var i = 0;i < data.length-10;i++){
        var index = Math.ceil(Math.random()*10);
        newData += data[index];
    }
    return newData;
}
// 创建一个全局变量
var sessionJson = {}
if(验证成功){
 var sessionId = RandomString();
 res.cookie('sessionId', sessionId);
sessionJson[sessionId] = oldUserName;
var timestamp = Date.parse(new Date());
sessionJson['timestamp'] = timestamp;
console.log(sessionJson);
}
// 销毁全局变量，根据时间戳判断，定时销毁
 if( timestamp > sessionJson.timestamp + 100000){
        // console.log('TOO LONG');
        sessionJson = {};
    }
```

方式二：采用文件存储，以及文件销毁

```javascript
var fs = require('fs');

// 把 sessionId 存到 demo 文件中
fs.writeFile("demo" , JSON.stringify(sessionJson), function(err) {
                    if(err) {
                        return console.log(err);
                    }
                
                    console.log("The file was saved!");
                });
// 定时清理文件内容
if( timestamp > sessionJson.timestamp + 100000){
        // console.log('TOO LONG');
        fs.truncate("demo" , 0, function(err) {
            if(err) {
                return console.log(err);
            }
            console.log("The file was CLEAR!");
        });
    }
```



