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
  > b. IE6双边距bug display：inline
  >
  > c. 在ie6，ie7中元素高度超出自己设置高度。原因是IE8以前的浏览器中会给元素设置默认的行高的高度导致的
  >
  > d. min-height在IE6下不起作用
  >
  > e. 透明性IE用filter:Alpha(Opacity=60)，而其他主流浏览器用 opacity:0.6
  >
  > f. input边框问题，去掉input边框一般用border:none;就可以，但由于IE6在解析input样式时的BUG(优先级问题)，在IE6下无效