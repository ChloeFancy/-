# text-align:justify的应用
在做一个日历的布局的时候，突发奇想，想用文本两端对齐来实现“一二三四五六日”的显示，但对于text-align:justify并不熟悉，所以踩了坑。<br>
## text-align:justify说明
首先，text-align属性是应用在块级元素上的，最常用的值是left、right、center。<br>
其次，justify的特殊之处在于：只有文本为多行时，才能发挥其作用，并且最后一行的显示效果相当于水平居左的。<br>
这也就是当文本只用一行时，text-align:justify看上去毫无效果的原因，因为这唯一的一行文本既是第一行也是最后一行。<br>
那么首先的解决方法就是给文本增加一行来作为最后一行。<br>
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
效果图如下：
