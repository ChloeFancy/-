# text-align:justify的应用
在做一个日历的布局的时候，突发奇想，想用文本两端对齐来实现“一二三四五六日”的显示，但对于text-align:justify并不熟悉，所以踩了坑。<br>
## text-align的值
* left 左对齐
* right 右对齐
* start The same as left if direction of block container is left-to-right and right if that is right-to-left.
* end 同理start
* jutify 两端对齐，最后一行采用start
## text-align:justify说明
首先，text-align属性是应用在块级元素上的，最常用的值是left、right、center。<br>
其次，justify的特殊之处在于：只有文本为多行时，才能发挥其作用，并且最后一行的显示效果相当于水平居左的。<br>
这也就是当文本只用一行时，text-align:justify看上去毫无效果的原因，因为这唯一的一行文本既是第一行也是最后一行。<br>
## 解决方法
1. 那么首先的解决方法就是给文本增加一行来作为最后一行。<br>
```html
<div>一二三四五六日</div>
```
```css
div{
  width: 200px;
  padding: 0 10px;
  border: 1px solid #666;
  text-align: justify;
}
div:after{
  content: ".";
  width: 100%;
  display: inline-block;
  /*将content隐藏起来*/
  height: 0;
  overflow: hidden;
}
```
* 为什么必须用“display: inline-block;”？<br>
因为:after伪元素本身是inline元素，但是需要设置其高度，所以把display的值改为inline-block。<br>
* width设置的问题<br>
不一定是100%，只要前面的文字行框的width+after伪元素的width>200px即可，目的是产生第二行。<br><br>
效果图如下：<br>![](https://github.com/ChloeFancy/My-Blog/blob/master/images/textAlign-justify.png)  
缺点:<br>
为了效果新增了一行，但是也占据了一行的高度。给div加上border就很明显了。<br>
2. 第二个解决方法就是让最后一行两端居中<br>
利用text-align-last属性<br>
```css
div{
  width: 200px;
  padding: 0 10px;
  border: 1px solid #666;
  text-align-last: justify;
}
```
效果图如下：<br>![](https://github.com/ChloeFancy/My-Blog/blob/master/images/textAlignLast-justify.png)  
