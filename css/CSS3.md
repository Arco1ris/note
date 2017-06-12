### CSS3

1. Flexbox 伸缩盒模型 
   
   主要思想：容器有能力让其子项目能够改变其宽度，高度等，以最佳方式填充可用空间（主要是为了适应所有类型的显示设备和屏幕大小）。Flex容器会使子项目（伸缩项目）扩展来填满可用空间，或缩小它们以防止溢出容器。
   
2. transition:过渡（平滑地改变CSS值）
   
   1. 从a点到b点，有时间，连续性，一般针对常规CSS属性
      
   2. 平滑地改变CSS值，无论是点击事件，焦点时间，还是鼠标hover，只要值改变了，就是平滑的
      
   3. 属性：
      
      `transition-property` ：指定过渡的性质，比如`tranition-properst:background`就是指`background`参与这个过渡，如果写`all`,那所有的属性都参与过渡
      
      `transition-duration`：指定这个过渡的持续时间
      
      `transition-delay`：延迟过渡时间
      
      `transition-timing-function`：指定过渡类型，有ease,linear(线性),ease-in(先慢后快),ease-out(先快后慢),ease-in-out(先慢后快再慢),cubic-bezier(贝塞尔曲线)
   
3. transform:变换
   
   1. 拉伸，压缩，旋转，偏移
      
   2. CSS3的2D transform函数包含了translate(),scale(),rotate(),skew()。
      
      translate()函数接受css的标准度量单位；
      
      scale()函数接受一个0到1之间的十进制值；
      
   3. ​
   
   ​
   
   ``` html
   div{
     transform:rotate(30deg);
   }
   ```
   
   ​