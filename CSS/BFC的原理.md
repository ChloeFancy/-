# BFC的原理
---

昨天在写两栏布局的时候，自己根据之前学习的三栏布局，写了5种方法，主要以控制自适应元素的margin或padding，并且将左边元素利用margin-left和相对定位等方法拉到特定位置上，这5种方法的一个共同特点都是需要计算自适应元素的宽度（利用了calc）或者使用CSS3中的box-sizing，在网上看了别人的写法（BFC+flaot）后，发现很久都没想起来BFC了，于是趁这一波复习一下。

---
### BFC的定义

Box Formatting Context，块级格式化上下文，是元素的一种特性，与IFC（Inline Formatting Context）并列在W3C CSS2.1的规范中。
有BFC特性的元素就相当于一个独立的box，在内部的子元素有独特的定位规则，并且子元素对对外部没有影响。BFC属于定位方案（普通流、绝对定位、浮动）中的普通流。

### BFC的触发条件
* 根元素
* position: absolute/fixed的元素
* float: left/right的元素
* overflow: auto/hidden/scroll(以及其他几个不是auto的值)的元素
* display: inline-block/tabel-cell/flex...
以上列出常见的供参考。

### BFC的布局规则
* 首先，计算有BFC特性的元素高度时，会计算里面的浮动元素高度在内。应用：清除浮动。
* 其次，在里面的块级元素从上到下排列，并且margin-bottom、margin-top会像相片的塑封边框一样重叠起来。
* 然后，有BFC特性的元素不会与浮动元素发生重叠。应用：两栏布局。
* 最后，内部子元素的定位和排列方式对于外部没有任何影响。

### BFC的应用

 1. 两栏布局
```css
aside{
    width: 200px;
	float: left;
    background-color: lightblue;
	line-height: 50px;
}
main{
	/*BFC*/
	overflow: auto;
	background-color: lightpink;
	line-height: 200px;
}

```
BFC不和浮动元素重叠
![](https://github.com/ChloeFancy/My-Blog/blob/master/images/BFC.png)  
与浮动元素重叠的情况
![](https://github.com/ChloeFancy/My-Blog/blob/master/images/no-BFC.png)  

 
 2. 清除浮动<br>
 利用BFC元素在计算高度时也会计算浮动的子元素高度的特点，我们可以让包含浮动元素的元素具有BFC属性。

```css
div.floatLeft{
	width: 200px;
	height: 300px;
	float: left;
	margin: 5px;
	background-color: lightblue;
}
div.clear{
	overflow: hidden;
}
div.wrapper{
	border: 1px dashed black;
	padding: 5px;
}
```
![](https://github.com/ChloeFancy/My-Blog/blob/master/images/clear.png)
![](https://github.com/ChloeFancy/My-Blog/blob/master/images/no-clear.png)<br>
 3. 防止上下margin重叠<br>
 首先了解margin上下重叠的计算方法。<br>
a. 如果margin-bottom和margin-top都是正数或都是负数，则两者看上去的margin效果为绝对值大者。<br>
都是正数的情况<br>
![](https://github.com/ChloeFancy/My-Blog/blob/master/images/margin-merge.png)  <br>
都是负数的情况<br>
![](https://github.com/ChloeFancy/My-Blog/blob/master/images/margin-nag-pos.png) <br>
b. 如果一正一负，结果是两者相加的和。<br>
![](https://github.com/ChloeFancy/My-Blog/blob/master/images/margin-neg.png) <br>

```css
.outer{
	overflow: hidden;
	border: 1px dashed black;
}
.inner{
	margin: 10px;
	width: 150px;
	height: 150px;
	line-height: 150px;
	text-align: center;
	background-color: lightblue;
}
```


### 总结
BFC有这么多的特点主要是归功于它既不受外部影响，又不影响外部。如果BFC元素与外部的浮动元素重叠，那么内部元素的定位就会受影响；如果BFC元素的高度计算忽略内部的浮动元素，那么外部元素的布局就会受到影响。（感觉自己离offer又近了一步，嘻嘻）
