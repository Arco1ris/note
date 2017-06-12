NPM 和 Yarn 的区别

1. npm 可能会导致两个机器的 package.json 文件相同但是安装的依赖版本不一致（因为有时候版本是某个范围而不是具体的版本号）， yarn 使用 yarn.lock 文件，依赖的版本是确定的。
2. yarn 安装为并行执行，速度很快
3. npm 的输出很冗余，执行 npm install <package> 的时候会出来很多内容，而 Yarn 只会有四句输出（MAC 系统）（Resolving packages,Fetching packages,Linking dependencies,Building fresh packages）
4. CLI 不同，yarn 的很多命令和 npm  不同

javascript 相关

##### [keymirror](https://www.npmjs.com/package/keymirror)

```javascript
//作用：创建一个值和键相等的对象
var KeyMirror = require('KeyMirror');
var Colors = KeyMirror({blue:null,red:null});
console.log(Colors); // {blue:'blue',red:'red'}  值和键相等
var myColor = Colors.blue;
console.log(myColor);  // blue 获取到的 Color 的值
var isColorValid = !!Colors[myColor]; // 由于值和键一样，所以可以拿值当做键来获取值，也可以看成拿到键名，用 ！将内容转换成 boolean 类型
console.log(isColorValid); // true

// 疑问：有什么意义？可以用在哪里？？
1.创建常量集合的时候可以用。
2.可以检查值的合法性？？
3.获取最原始的 key ？？
```

##### [classnames](https://www.npmjs.com/package/classnames)

```javascript
// 有选择性的将 classNames 放在一起，
// classNames 函数可以接受任意数量的参数，参数可以是字符串或者对象，比如参数 'foo' 是 {foo:true} 的缩写，如果参数的值是 falsy，它将不会被输出。
var classNames = require('classnames');
classNames('foo', 'bar'); // => 'foo bar' 
classNames({'foo-bar':true}); // => 'foo-bar'
classNames({'foo-bar':false}); // => ''
```

疑问：这个库有什么意义，可以用在哪里

写公共组件的时候可以用

```javascript
// 普通的写法
var Button = React.createClass({
  render () {
    var btnClass = 'btn';
    if(this.state.isPressed) btnClass += 'btn-pressed';
    else if (this.state.isHovered) btnClass += 'btn-hover';
    return <button className={btnClass}>{this.props.label}</button>;
  }
})
// 用这个库的写法
var classNames = require('classnames');
var Button = React.createClass({
  render () {
    var btnClass = classNames({
      'btn':true,
      'btn-pressed':this.state.isPressed,
      'btn-hover':!this.state.isPressed && this.state.isHovered
    });
    return <button className={btnClass}>{this.props.label}</button>
  }
})
```

```javascript
// 备用版本，可以删除重复或者 falsy 的值, 
// 但是 this version is slower(about 5x) so it is offered as an opti-in
var classNames = require('classnames/dedupe');
classNames('foo','foo','bar'); // => 'foo bar'
classNames('foo',{foo:false,bar:true}); // => 'bar'
```



##### [trim](https://www.npmjs.com/package/trim) 去掉字符串的空白

```javascript
trim('  foo bar  '); // 'foo bar'
trim.left('  foo bar  ');// 'foo bar  '
trim.right('  foo bar  ');// '  foo bar'
```

##### [underscore](https://www.npmjs.com/package/underscore)  JavaScript's functional programming helper library

提供函数式编程的方法，比如 map,each,reduce,filter 等等

 疑问：

1. 该库提供的 map,reduce 方法和 js 自带的原生 map,reduce 等有什么区别？

   原生的 map 是 Array 的方法，只能用在数组上，而该库的 map 还可以用在对象上或者组合数组等等，

2. underscore 和 lodash 有什么区别

   underscore 是函数式编程，参数能直接传 function，而 lodash 不是

3. ​


##### [rc-form](https://www.npmjs.com/package/rc-form) React High Order Form Component (React 高阶 form 组件)

```javascript
// antd 的组件
```

bluebird

一个 promise 库




