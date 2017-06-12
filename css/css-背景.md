## css

### 背景图像效果

### 背景图

1. 水平平铺 ：`background-repeat:repeat-x;`
   
2. 背景位置：`background-position:left center;`可以设置成left,centre之类的关键字，也可以设置实际像素，如果设置成实际像素，那么就是背景图片左上角距离父元素左上角的距离，比如指定垂直位置和水平位置都是10像素，那么图像的左上角将出现在距离父元素左上角下方10像素，左边10像素的位置。如果设置成百分比20%，就是图像上距离左上角20%的点定位到父元素距离左上角20%的地方。 所以若希望是垂直居中，则需要将垂直位置设置为50%：`background-positon:0 50%;`![Screen Shot 2015-11-25 at 5.19.58 PM](/Users/rainbow/Desktop/Screen Shot 2015-11-25 at 5.19.58 PM.png)
   
3. background简写版本：`background:#ccc url('/img.gif') no-repeat left centre` 。   
   
4. 滑门技术？？？？
   
5. 山顶角
   
6. 多背景图案
   
   ``` css
   .box{
     background-image:url(/img/a.gif),url(/img/b.gif),url(/img/c.gif);
     background-repeat:no-repeat,no-repeat,no-repeat;
     background-position:top left,top right,bottom left;
   }
   ```
   
7. `border-image`,border-image 可以根据一些简单的百分比规则把图像划分为9个单独的部分，浏览器会自动地使用适当的部分作为边框的对应部分。这种技术成为**九分法缩放（nine-slice scaling）**，它有助于避免在调整圆角框大小时通常会出现的失真。（p79）
   
8. 阴影部分，以前是使用大的投影图像应用于容器DIV的背景，然后使用负的外边距偏移图像。现在用`box-shadow`属性（垂直和水平偏移，投影的宽度，颜色）。
   
9. 不透明度可用RGBA设置，A表示不透明度，若为1则完全不透明，若为0则完全透明。如：`background-color:rgba(0,0,0,0.8)`。
   
10. PNG文件格式最大的优点一直是它支持alpha透明度。**？？**
    
11. 只让IE6看到新的样式表，可以添加以下代码：
    
    ``` html
    <!--[if ie 6]>
    	<link rel="stylesheet" type="text/css" href="ie6.css"/>
    <![end if]-->
    ```
    
    ​