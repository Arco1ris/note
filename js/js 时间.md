##### 时间格式到底有哪些，互相之间如何转换

**ISO Date**  "2015-03-25"  国际标准

**Short Date**   "03/25/2015" 

**Long Date** "Mar 25 2015" 或者  "25 Mar 2015" 

**Full Date** "Wednesday March 25 2015"

```javascript
var t = new Date("2015-03-25"); // Wed Mar 25 2015 08:00:00 GMT+0800 (CST)
var t = new Date(); //输出当前时间  Fri Dec 16 2016 11:55:16 GMT+0800 (CST) 
```

new Date();方法可以将前四种时间格式当做参数，输出 UTC 格式的时间

- "YYYY/MM/DD" and "DD-MM-YYYY"  这两种格式(注意年月日的顺序)是不规范的，有的浏览器会猜格式，有的浏览器则直接返回 NaN 或者 Invalid date;
- "MMM DD YYYY"（Mar 25 2015）和 "DD MMM YYYY"(25 Mar 2015) 都是合格的格式，并且月份可以写成全拼而非简写（March 25 2015），并且不关心大小写和逗号，（MARCH,25,2015）也能输出正确的 UTC

**Time Zones**（部分）

**UTC**  "2015-03-25T12:00:00" 看到日期和时间中间有一个字母 T 表示为 UTC 时间 是现在使用的标准时间

UTC 是时间标准；ISO-8601 是表示时间的一种标准格式

ISO-8601 的日的时间表示法，在 UTC 时间后加 Z，如 "2015-03-25T12:00:00Z"，Z 表示是通用标准，其它的时区的时间和 UTC 不同，所以用实际时间加时差表示。

**GMT** Greenwich Mean Time   "Mon Feb 13 08:00:00 GMT+08:00 2012" 中间有 GMT+ 字样

UTC（Universal Time Coordinated 原子钟提供）和 GMT (Greenwich Mean Time 格林尼治时间)一样

**EDT**  (US)Eastern Daylight Time  东部夏令时

**CDT**  (US) Central Daylight Time  中部夏令时

**MDT** (US)Mountain Daylight Time 山地夏令时

**PDT** (US) Pacific Daylight Time 太平洋夏令时

**EST** (US) Eastern Standard Time 东部标准时间

**CST**   "Web Mar 25 2015 08:00:00 GMT+0800"   中部标准时间

**MST** (US) Mountain Standard Time 山地标准时间

**PST** (US) Pacific Standard Time 太平洋标准时间



设置时间时，没有指定 time zone，js 将会使用浏览器的 time zone

获取时间时，没有指定 time zone，结果将会转换成浏览器的 time zone

总之，如果时间是用 `GMT` 时间创建，如果浏览器 from central US 时间将会被转换成 CDT。



##### 时间之间的转换

UTC,GMT,unix timestamp

- 获取当前时间，UTC

  ```javascript
  new Date()  // Fri Dec 16 2016 15:59:28 GMT+0800 (CST)
  ```


- 获取当前时间戳

  ```javascript
  // 方法一，貌似没多大用处
  Date.UTC(2016,12,16,23,59,59,999) 
  // 参数分别是，年月日时分秒毫秒 输出时间戳1484611199999

  // 方法二，推荐！！！
  + new Date()   // 输出当前时间戳 1481877029487 
  // A unary operator like plus triggers the valueOf method in the Date object and it returns the time-stamp (without any alteration). 一元运算比如+会触发 valueOf 方法作用到 Date 对象上，让它直接返回时间戳

  // 方法三
  new Date().getTime(); // 输出当前时间戳 1481877193145

  // 方法四
  Date.now() / 1000 | 0  // 1481877228 不包括毫秒

  // 方法五
  new Date().valueOf() // 输出当前时间戳 1481877193145
  ```

- timestamp => UTC or GMT

  ```javascript
  (new Date(timestamp)).toUTCString()  // "March, 06 Dec 2016 10:01:02 GMT"，此处使用 toGMTString() 结果是一样的

  new Date(unix_timestamp*1000); // Sat Oct 30 48928 08:24:39 GMT+0800 (CST)
  ```

- timestamp => Short Date  或者 UTC => Short Date 

  ```javascript
  var moment = require('moment');
  var dUTC = new Date();
  var formatTime01 = moment(dUTC).format('YYYY-MM-DD hh:mm:ss');

  var dUNIX = +new Date();
  var formatTime02 = moment(dUNIX).format('YYYY-MM-DD hh:mm:ss');

  // 上面的输出一致，均可返回需要的格式，返回格式根据 moment 语法定
  //ISO-8601 的表达日的时间格式同样可以用 moment 解决
  var dUNIX = '2016-12-16T12:23:45Z';
  var formatTime02 = moment(dUNIX).format('YYYY-MM-DD hh:mm:ss'); // 输出指定格式的时间

  ```


