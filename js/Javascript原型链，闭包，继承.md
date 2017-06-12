### 原型链和继承

1. 一切（引用类型）都是对象，对象是属性的集合。

2. 对象都是通过函数创建的。(函数是一种对象)

3. **每个函数**都有一个属性叫做prototype(原型)，属性值是一个对象（属性的集合）。

   该对象里默认包含两个属性：`constructor`和`__proto__` ,其中`constructor`指向这个函数本身。`__proto__`（隐式原型）指向原型（创建该对象的函数）的`prototype`。

   **公式一：`myFunction.prototype.constructor = myFunction  //true`**

4. 我们可以在 prototype 中新增自己的属性

   `myFunction.prototype.name = 'rainbow';`

5. 例：

   ``` javascript
   function Fn(){};//新建一个函数
   Fn.prototype.name = 'rainbow';
   Fn.prototype.age = 20;
   var fn = new Fn(); //创建一个实例对象
   fn.name;//输出rainbow
   ```

   解析：Fn 是一个函数, fn 对象是从 Fn 函数 new 出来的，这样 fn 对象就可以调用 Fn.prototype 中的属性。

   因为**每个对象**都有一个隐藏的属性`__proto__`，这个属性引用了创建这个对象的函数的 prototype。

   **公式二：`fn.__proto__===Fn.prototype`**

   原型的`prototype`和实例的`__proto__`值相等。 

6. 由于 function 的 prototype 是一个对象（属性的集合），原型是 Object，所以一般 function 的prototype对象的`__proto__`指向Object.prototype，Object.prototype 是个特例，它的`__proto__`指向的是null。

7. 函数 function 都是由 Function（注意大写）创建的。`var myFun = new Function()`，所有的 function 实际上都是 Function 的实例。

8. **公式三：`Object.__proto__ ===Function.prototype`**

   Function 是 Object 的原型

9. **公式四：`myFun.__proto__===Function.prototype`**

   Function 是自定义 function 的原型

10. **公式五：`Function.__proto__===Function.prototype`**

  Function 是 Function 的原型，也就是Function 被自身创建，此处构成了一个环形

11. **公式六：`myFun.prototype.__proto__===Object.prototype`**

   自定义函数的prototype的`__proto__`指向 Object.prototype

12. **公式七：**`myFun.constructor === Function`

   实例的 `__proto__ ` 指向原型的 `prototype`，所以它`constructor` 属性是指向原型的，可参见公式一。（一般不需要加`__proto__`，因为这是它的隐藏属性，顺着`__proto__`查找属性时，不需要添加`__proto__`）

13. Function.prototype 指向的对象，它的 `__proto__` 指向Object.prototype。有待考证。

14. ``` javascript
      function A(){}
      var a1 = new A()
      a1 instanceof A //true
      a1 instanceof Object //true
      ```
    ​```
    ​```
    
    Instanceof 的判断规则：沿着`__proto__`这条线查找，同时沿着`prototype`这条线来找，如果两条线能找到同一个引用，即同一个对象，那就返回true，如果找到终点还未重合，则返回 false。
    
    此处解释可不看：a1的`__proto__`指向A的`prototype`，A的`prototype`的`__proto__`是指向Object的prototype。
    ​```

3.  **访问一个对象的属性时，先在基本属性中查找，如果没有，再沿着`__proto__`这条链向上找，这就是原型链。**

4.  区分一个属性是基本属性还是从原型中查找，可以使用hasOwnProperty.（属于Object.prototype），**由于所有对象的原型链都会找到Object.prototype，所以所有对象都会有Object.prototype的方法。**

5.  每个函数(函数原型)都有 call,apply 方法，都有length,arguments,caller等属性，是因为函数都是由 Function 函数创建，因此会继承 Function.prototype 中的方法。（这些原型同时也会继承 Objet.prototype 的方法和属性，但是由原型生成的实例只继承 Object.prototype 的方法和属性）

### 执行上下文

1. 通俗定义：在代码执行之前，把将要用到的所有的变量都事先拿出来，有的直接赋值了，有的先用 undefined 占个空。

2. 无论在哪个位置获取 this，都是有值的。

3. 浏览器准备工作：1.对变量进行声明（而不是赋值，赋值是在赋值语句执行的时候进行的，变量声明，默认赋值为 undefined）；2.给 this 赋值 ；3.给函数声明赋值，类似情况2（比如：`function f1(){}`就是函数声明），给函数表达式声明，类似情况1（比如：`var f2 = function(){}`就是函数表达式），**总结：对函数声明和 this 赋值,对变量和函数表达式等进行声明。**以上几种数据的准备情况称为“执行上下文”

4. javascript 在执行一个**代码段**之前，都会进行这些“准备工作”来生成执行上下文，这个“代码段”分成三种情况：全局代码，函数体，eval 代码。

   所谓“代码段”就是一段文本形式的代码，全局代码是写在`<script>`标签里的代码段，而eval代码接受的也是一段文本形式的代码。函数体是代码段是因为函数在创建时，本质上是 new Function(…) 得来的，其中需要传入一个文本形式的参数作为函数体。

5. **函数每被调用一次，都会产生一个新的执行上下文环境。**因为不同的调用可能就会有不同的参数。

6. **函数在定义的时候（不是调用的时候），就已经确定了函数体内部自由变量的作用域。** 

   ``` javascript
   var a = 10;
   function fn() {
   	console.log(a);//a 是自由变量
     					// 函数创建时，就确定了 a 要取值的作用域
   }
   function bar(f) {
     var a = 20;
     f(); // 打印 "10"，而不是 "20" 
   }
   bar(fn);
   ```

   ​

7. 上下文环境的数据内容总结

   | 普通变量（包括函数表达式），如 var a = 10; | 进行声明（默认赋值为 undefined） |
   | --------------------------- | --------------------- |
   | 函数声明，如 function fn() {}     | 进行赋值                  |
   | this                        | 进行赋值                  |

   如果代码段是函数体，附加上:

   | 参数          | 赋值   |
   | ----------- | ---- |
   | arguments   | 赋值   |
   | 对自由变量的取值作用域 | 进行赋值 |

   ​

8. this 

   **函数中this到底取何值，是在函数真正被调用执行的时候确定的，函数定义的时候确定不了。**因为this的取值是执行上下文环境的一部分，每次调用函数，都会产生一个新的执行上下文环境。

   this 的取值分为四种情况：

   1. 构造函数

      如果函数为构造函数用，那么其中的this就代表它即将new出来的对象。

      ``` javascript
      function Foo(){
        this.name = "rainbow";
        console.log(this); 
      }
      var foo = new Foo();//{name:"rainbow"}
      ```

      如果直接调用函数Foo()，this则代表了window

   2. 函数作为对象的一个属性

      函数作为一个对象的属性并且被调用时，this指向该对象。

      ``` javascript
      var Foo = {
        x: 10,
        fn: function(){
              console.log(this); //Object {x:10,fn:function}
      		console.log(this.x); //10
      	}	 
      }
      Foo.fn(); // 作为对象的属性被调用
      var fn1 = Foo.fn; // 并没有作为对象的属性被调用，this指向window
      fn1(); 
      ```

      fn 作为 Foo 的属性且被调用，this 指向 Foo 。

   3. 函数用call或者apply调用

      当函数被 call 或者 apply 调用时，this 的值就取传入的对象的值。

      ``` javascript
      var obj = {x:10};
      var fn = function() {
        console.log(this);
      };
      fn.call(obj); // fn 调用了obj的属性
      ```

   4. 全局 & 调用普通函数

      全局情况下，this永远是window，普通函数在调用时，其中的this也都是window

      ``` javascript
      function foo(){
        console.log(this);
      }
      foo(); //window{...}
      ```

9. 执行上下文栈

   执行全局代码时，会产生一个执行上下文环境，每次调用函数都又会产生执行上下文环境。当函数调用完成时，这个上下文环境以及其中的数据都会被消除，再重新回到全局上下文环境。**处于活动状态的执行上下文环境只有一个。**

10. 作用域

  “Javascript 没有块级作用域”

  javascript 除了全局作用域之外，只有函数可以创建的作用域。我们在声明变量时，全局代码要在代码前端声明，函数中要在函数体一开始就声明好。其它地方都不要出现变量声明，而且建议用“单 var”形式。

  作用域最大的用处就是隔离变量，不同作用域下同名变量不会有冲突。作用域在函数定义时就已经确定了，而不是在函数调用时确定。同一个作用域下，不同的调用会产生不同的执行上下文环境，继而产生不同的变量的值。所以，作用域中变量的值是在执行过程中产生的确定的，而作用域却是在函数创建时就确定了。

11. 从自由变量到作用域链

   自由变量：在 A 作用域中使用的变量 x，却没有在 A 作用域中声明（在别的作用域中声明的），对于 A 作用域来说，x 就是自由变量。

   比如：

``` javascript
   var x = 10;
   function fn() {
     var b = 20;
     console.log(x+b); //此处 x 是自由变量
   }
   // 在调用fn()函数的时候，b的值可以直接在fn作用域中取，而x需要到另外一个作用域取，要在创建fn函数的作用域中取值，无论fn函数在哪里被调用。所以此处在全局作用域中取x的值。
```

   **要到创建这个函数的作用域中取值。** 是创建，而不是调用。

1.  ``` javascript
     var arr2 = function(){
      var x=[]; 
      arr.map(function(item){
        x.push(item);
      });
      return x;
     }();
     ```