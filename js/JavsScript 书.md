1. JavaScript 的起源
   
   它是 Netscape 公司和 Sun 公司合作开发的。
   
2. DOM 
   
   DOM 是一套对文档的内容进行抽象和概念化的方法
   
   W3C 对 DOM 的定义：一个与系统平台和编程语言无关的接口，程序和脚本可以通过这个接口动态地访问和修改文档的内容，结构和样式。
   
3. 最好的做法是把`<script>`标签放在 HTML 文档的最后，`<body>`标签之前。
   
4. 程序设计语言分为解释型和编译型， JAVA 或 C++ 等语言需要一个编译器（conpiler）。编译器是一种程序，能够把 JAVA 等高级语言编写出来的源代码翻译为直接在计算机上执行的文件。解释型语言不需要编译器，而需要解释器。对于 JavaScript 语言，在互联网环境下，Web 浏览器负责完成有关的解释和执行工作。浏览器中的 JavaScript 解释器将直接读入源代码并执行。浏览器中如果无解释器，JS 代码将无法执行。所以编译型语言的代码有错误，这些错误能在编译阶段就被发现，而解释型语言代码中的错误只能等到解释器执行到有关代码时才能被发现。
   
5. **驼峰式**是函数名、方法名和对象属性名命名的首选格式。**下划线**用来命名变量。
   
   比如：myReport 是函数名，my_report 是变量名。
   
6. 必须明确类型声明的语言称为强类型语言。JS 不需要进行类型声明，所以是弱类型语言，意味着程序员可以在任何阶段改变变量的数据类型。
   
7. 数据类型
   
   1. 字符串 string 
      
      遇到单双引号嵌套的情况需要反斜杠转义。
      
   2. 数值 Number  （浮点数，整数，负数等等）
      
   3. 布尔值 Boolean
      
      布尔值不是字符串，'true' 是字符串，true 是布尔值
      
   4. 数组 Array
      
      字符串，数值和布尔值都是标量（scalar），它在任意时刻就只能有一个值。
      
      数组用 Array 声明，同时还可以指定数组的初始元素个数，也就是数组的长度。数组中可以存入布尔值，其它数组等等。
      
   5. 对象
      
      对象不像数组用下标获取值，而是用点获取属性。  
   
8. 操作
   
   算术符：加减乘除
   
   条件语句 : `if(something)` 和 `if(somthing != null)` 完全等价
   
   比较操作符： === 表示严格相等，会同时比较数值和数据类型，== 会转换数据类型并比较数值
   
   逻辑操作符：&& ，|| ，！
   
   循环语句：while,do..while,for 
   
9. 函数
   
   如果一个函数能把结果返回给调用函数的语句，需要用到 return 语句。
   
10. 对象
    
    对象是一种非常重要的数据类型，对象是自包含的数据集合，包含在对象里的数据可以通过两种形式访问— 属性和方法。
    
    属性是隶属于特定对象的变量。
    
    方法是只有某个特定对象才能调用的函数。
    
    **对象就是由一些属性和方法组合在一起而构成的一个数据实体。**
    
    创建实例：
    
    `var Alice = new Person;`
    
    内建对象：比如 Array，Math，Date 等等
    
    宿主对象：这些对象不是由 JS 语言本身而是由它的运行环境提供的，具体到Web 应用，这个环境就是浏览器。由浏览器提供的预定义对象被称为宿主对象。包括：From, Image, Element, Element 等。
    
11. 节点
    
    文档是由节点构成的集合。有文本节点，元素节点，属性节点等等。
    
    文档几乎每一样东西都是一个节点，甚至连空格和换行符都会被解释为节点，它们也全部包含节点该有的属性。
    
12. 常用的 DOM 操作
    
    1. getElementById
       
    2. getElementsByTagName ：允许把通配符作为参数，如 `getElementsByTagName("*")`表示选取所有的标签
       
    3. getElementsByClassName：要使用多个类名，只需要在参数中用空格分开即可
       
    4. getAttribute：它不属于 document 对象，所以不能通过 document 对象调用，它只能通过元素节点对象调用。
       
       如：
       
       ``` javascript
       var paras = document.getElementsByTagName("p");
       for(var i = 0; i < paras.length; i++) {
         alert(paras[i].getAttribute("title"));
       }
       ```
       
    5. setAttribute：同样只能用于节点元素，包含两个参数，第一个是属性名，第二个是属性值。
    
    **严格遵守“第一级 DOM ”能够让你避免与兼容性有关的任何问题**
    
13. 事件处理函数
    
    作用：在特定时间发生时调用特定的 JS 函数。
    
    机制：某个元素被添加了事件处理函数后，一旦事件发生，相应的JS代码就会得到执行。被调用的JS代码就会得到执行。被调用的JS代码可以返回一个值，这个值被传递给了那个事件处理函数。（所以我们通常在最后 `return false;`以保证阻止默认事件和冒泡事件）
    
    onmouseover()
    
    onmouseout()
    
    onclick()
    
    unload()
    
    标签中调用事件处理函数，参数用this表示当前的标签元素。比如：
    
    `<p onclick="clickMe(this)">this is a text</p>`
    
14. 属性：
    
    1. childNodes属性：可以用来获取**任何一个元素的所有子元素**
       
       比如：获取body元素的全体子元素
       
       `document.getElementByTagName("body")[0].childNodes`
       
    2. nodeType 属性：返回节点的属性，但是返回值是数字而不是任何字符。
       
       **元素节点**的 nodeType 属性值是1
       
       **属性节点**的 nodeType 属性值是2
       
       **文本节点**的nodeType 属性值是3
       
    3. nodeValue 属性：用来得到和设置一个节点的值
       
       注意，`<p>`标签本身是一个节点，它的 nodeValue 属性是一个空值，`<p>`里的文本是另一种节点，它是标签的子节点，所以我们为了得到文本值，需要这样写:
       
       `document.getElementsByTagName("p").childNodes[0].nodeValue`
       
    4. firstChild 和 lastChild
       
       node.firstChild 等价于 node.childNodes[0]
       
       node.lastChild 等价于 node.childNodes[node.childNodes.length - 1]
    
15. 如果用户浏览器不支持JS怎么办？
    
    **平稳退化：**意思是虽然某些功能无法使用，但最基本的操作仍然能完成。
    
    JavaScript 使用 window 对象的 open() 方法来创建新的浏览器窗口。
    
    如：
    
    ``` javascript
    function popUp(winURL) {
      window.open(winURL,"popup","width=320,height=480");
    }
    ```
    
    不推荐的两种做法：因为以下两种方法无法平稳退化，一旦用户禁用浏览器的JS功能，这样的链接将毫无用处。
    
    1. "javascript:"伪协议 ：
       
       `<a href="javascript:popUp('http//...');">Example</a>`
       
    2. 内嵌事件处理函数
       
       `<a href="#" onclick="popUp('http//...');return false;">Example</a>`
    
    只有少数爬虫能理解JS，如果你的 JS 网页不能平稳对话，它们将在搜索引擎上的排名就会大受损害。
    
    平稳退化的一个例子：不过在标签内使用 onclick 等函数并不是一个好办法
    
    `<a href="http//..." onclick="popUp(this.href);return false;"></a>`
    
16. **渐进增强**
    
    在保证大多数浏览器都能访问的情况下，使用一些方式让部分支持的浏览器会有更好的用户体验。
    
17. **向后兼容**
    
    1. 对象检测：`if(document.getElementById)`检测是否支持 getElementById 这个方法。或者 `if(!document.getElementById) {return false;}`
    2. 浏览器嗅探技术（历史技术，不做介绍）
    
18. **性能考虑**
    
    1. 尽量少访问 DOM 和尽量减少标记。因为不管什么时候，只要是查询 DOM 中的某些元素，浏览器都会搜索整个 DOM 树，从中查找可能匹配的元素。尽量减少文档中的标记的数量，因为过多不必要的元素只会增加 DOM 树的规模，从而增加遍历 DOM 树以查找特定元素的时间。
    2. 合并和放置脚本。将多个文件合并成一个文件，以减少请求次数；将脚本放在<head>中会导致浏览器无法并行加载其它文件，会造成阻塞，影响 DOM 树加载。所以要把脚本放在文档最后，能让页面加载
    3. 压缩脚本。把不必要的字节，空格注释等都统一删除，从而达到“压缩”的目的。
    
19. 共享 unload() 事件
    
    addLoadEvent ：只有一个参数，表示打算在页面加载完毕时执行的函数名称
    
20. 键盘访问：
    
    onkeypress() 
    
21. document.write()
    
    innerHTML()
    
    createElement()
    
    createTextNode()
    
    appendChild()
    
    insertBefore()
    
22. CSS:
    
    className 属性
    
    ``` javascript
    var para = document.getElementById("example");
    para.style.color = "black";
    // fontSize,backgoundColor
    ```
    
    ​
    
    ​
    
    ​
    
    ​
    
    ​
    
    ​
    
    ​









 









