**垂直（上下）居中的方式：**

前提条件：

``` css
<div id="parent">
	<div id="child">child</div>
</div>

#parent{
  width:200px;
  height:200px;
  border: 1px solid black;
}
#child{
	width:100px;
    height:100px;
    border: 1px solid black;
}
```



1. ``` css
   #parent {
     position:relative;
   }
   #child {
     position:absolute;
     top:50%;
     margin:-50px 0 0 0; /* 这个值要取div高度的一半*/
   } 
   ```

2. ``` css
   #parent{
     position: flex;
   }
   #child {
     margin: auto;
     /*或者*/
     align-items: center; 
   }
   ```

3. ``` css
   /*这里子元素将会尽可能的填充父元素,内部文字会居中*/

   #parent {
     display: table;
   }
   #child {
     display: table-cell;
     vertical-align:middle;
   }
   ```

4. ``` css
   #parent {
     position: relative;
   }
   #child {
       position: absolute;
   	bottom: 0;
   	top: 0;
   	margin: auto;
   }
   ```

水平居中：

1. ``` css
   /*子元素 div*/
   #child {
     margin: 0 auto;
   }
   ```

2. ``` css
   /*父元素 让父元素中的文字水平居中*/
   #parent {
     text-align:center;
   }
   ```

比较：

1. text-align: 是设置文本的对齐方式，写在包裹文本的标签上
2. vertical-align: 设置图片的对齐方式，没有 center 属性值。



##### 内联元素居中

水平居中：

1. 行内元素：`text-align:center`
2. flex 布局：`display:flex;justify-content:center`

垂直居中设置：

1. 父元素高度确定的单行文本（内联元素）：`height:line-height`
2. 父元素高度确定的多行文本（内联元素）
   + 插入 table，然后设置 vertical-align:middle
   + 设置：`display:table-cell;vertical-align:middle`

##### 块级元素居中

水平居中：

1. 定宽块状：`margin:auto`

2. 不定宽：

   + 在元素外加入 table 标签（完整的 table,tbody,tr,td），该元素写在 td 内，然后设置 margin 的值为 auto

   + 给该元素设置 `display:inline`

   + 父元素设置：`position:relative;left:50%`

     子元素设置：`position:relative;left:50%`

垂直居中

1. ```css
   /*使用position:absolute（fixed）,设置left、top、margin-left、margin-top的属性;*/
   .box{
   position:absolute;/*或fixed*/
   top:50%;
   left:50%;
   margin-top:-100px;
   margin-left:-200px;
   }
   ```

2. ```css
   /*利用position:fixed（absolute）属性，margin:auto这个必须不要忘记了;*/
   .box{
       position: absolute;或fixed
       top:0;
       right:0;
       bottom:0;
       left:0;
       margin: auto;
   }
   ```

3. ```css
   /*利用display:table-cell属性使内容垂直居中;*/
   .box{
       display:table-cell;
       vertical-align:middle;
       text-align:center;
       width:120px;
       height:120px;
       background:purple;
   }
   ```

4. ```css
   /*使用css3的新属性transform:translate(x,y)属性;*/
   .box{
       position: absolute;
       transform: translate(50%,50%);
       -webkit-transform:translate(50%,50%);
       -moz-transform:translate(50%,50%);
       -ms-transform:translate(50%,50%);
   }
   ```

5. ```css
   /*最高大上的一种，使用:before元素;*/
   .box{
       position:fixed;
       display:block;
       background:rgba(0,0,0,.5);
   }
   .box:before{
       content:'';
       display:inline-block;
       vertical-align:middle;
       height:100%;
   }
   .box.content{
       width:60px;
       height:60px;
       line-height:60px;
     	color:red;
   }
   ```

6. flex 布局