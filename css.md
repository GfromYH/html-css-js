## Css tips



+ line-height三种赋值方式有何区别？（带单位、纯数字、百分比）

  > 带单位：px不用计算，em则会使元素以其父元素font-size值为参考来计算自己的行高
  >
  > 纯数字：把比例传递给后代，例如父级行高为1.5，子元素字体为18px，则子元素行高为1.5*18=27px
  >
  > 百分比：将计算后的值传递给后代

+ :link、:visited、:hover、:active的执行顺序是怎么样的？

  > L-V-H-A，l(link)ov(visited)e h(hover)a(active)te

+ 经常遇到的浏览器兼容性有哪些？

  > a. 浏览器默认的margin和padding不同
  >
  > b. IE6双边距bug display：inline，造成原因：1.块元素 2.浮动 3.设置外边距
  >
  > c. 在ie6，ie7中元素高度超出自己设置高度。原因是IE8以前的浏览器中会给元素设置默认的行高的高度导致的
  >
  > d. min-height在IE6下不起作用
  >
  > e. 透明性IE用filter:Alpha(Opacity=60)，而其他主流浏览器用 opacity:0.6
  >
  > f. input边框问题，去掉input边框一般用border:none;就可以，但由于IE6在解析input样式时的BUG(优先级问题)，在IE6下无效
+ rem,em,px区别

  > 1）px像素（Pixel） 。绝对单位。像素px是相对于显示器屏幕分辨率而言的，是一个虚拟长度单位，是计算机系统的数字化图像长度单位，如果px要换算成物理长度，需要指定精度DPI。
  >
  > 2）em是相对长度单位，相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸。它会继承父级元素的字体大小，因此并不是一个固定的值，总结em首先会看本身是否有font-size，如果没有，则继承父类的font-size。
  >
  > 3）rem是CSS3新增的一个相对单位（rootem，根em） ，使用rem为元素设定字体大小时，仍然是相对大小，
  >
  > 但相对的只是HTML根元素。

