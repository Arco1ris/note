### css

#### 链接

1. 简单的链接样式
   
   ``` html
   <p>
     <a href="#mainContent">Skip to main content</a>//当用户单击此锚点时，会跳转到另外一个锚点
   </p>
   <h1>
     <a name="mainContent">Welcome</a>
   </h1>
   ```
   
2. 伪类选择器
   
   `:hover`，`:focus`，`:active`:寻找被激活的元素，（当链接被点击时），`:visited`。
   
   不仅`<a>`标签可以使用这些，别的元素同样可以使用这些伪类选择器，比如提交按钮，表格行。
   
   注：IE7及更低版本不支持在链接以外的其它元素上使用伪类选择器。
   
3. 奇特的链接下划线
   
   ``` css
   a:link,a:visited,a:hover,a:active{
     color:#666;
   	text-decoration:none;
     background:url(/img/a.png) repeat-x left bottom;//试验失败
   }
   ```
   
4. 为链接目标设置样式
   
   在链接末尾加上#字符，再加上要链接元素的ID，可以直接跳转到该元素
   
   ``` html
   <a href="http://example.com/story.html#comment3">A great comment by Simon</a>
   ```
   
   但是页面内容过多时，很难看出链接到底跳转到了哪个元素，为了解决这个问题，CSS3允许使用`:target`伪类为目标元素设置样式
   
   ``` css
   .comment:target{
     background-color:yellow; 
     //background-image:url(imh/fade.gif);或者直接使用动态图片
   }
   ```
   
5. 突出显示不同类型的链接
   
   为了让跳转到其它网站的链接与跳转到当前站点其它页面的链接看起来不一样，可以在外部链接旁加一个小图标：
   
   ``` css
   .external{
     background:url('图标链接') no-repeat right top; //加图片都失败了
     padding-right:10px;
   }
   a[href$=".pdf"]{ //为PDF专门设置的样式
     background:url('img/pdflink.gif') no-repeat right top;
     padding-right:10px;
   }
   ```
   
   注：IE6和更低版本不支持属性选择器
   
6. 创建类似按钮的链接
   
   有时候为了让链接看起来有按钮的效果，有更大的可单击区域，可将`display`属性设置为`block`,然后修改`width`,`height`和其他属性创建需要的样式和单击区域。如下：
   
   ``` css
   a{
     display:block;
     width:6.6em;
     line-height:1.4;
     text-align:center;
     text-decoration:none;
     border:1px solid black;
     background-color:#666;
     color:#fff;
   }
   ```
   
   ​