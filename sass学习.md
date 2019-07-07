## Sass

### Sass安装环境

~~~javascript
//需要在ruby环境下
ruby -v
//查看gem包版本类似与node里自动安装的npm包管理
gem -v

gem install sass
//查看sass版本
sass -v

~~~

+ 编译

~~~javascript
//将scss映射成css
sass input.scss output.css

//随时监听sass变化
sass --watch input.scss:output.css
~~~

### sass使用

+ 变量命名

~~~javascript
$变量名：值

//如果定义变量在{}里，意味着sass有块作用域的概念
.nav{$width:100px;width:$width;height:100px}
~~~

+ 父类选择器&

~~~javascript
$blue:blue;
.blue{
  width: 100px;
  height: 100px;
  background-color: $blue;
  &:hover{
    background-color: #e0e044;
  }
}
~~~

+ 群组选择器

~~~javascript
$blue:blue;
.blue{
  width: 100px;
  height: 100px;
  background-color: $blue;
  &:hover{
    background-color: #e0e044;
  }
  h1,h2,h3{
    font-size: 16px;
  }
}
~~~

+ 默认变量

~~~javascript
$blue:blue !default;
~~~

### 混合器 @mixin

通过@mixin来定义混合器，通过@include来引用

~~~javascript
@mixin rounded-radius{
  -moz-border-radius: 5px;
  border-radius: 5px;
}
.blue{
  width: 100px;
  height: 100px;
  @include rounded-radius;
}
~~~

+ 混合器传参

~~~javascript
@mixin rounded-radius($size){
  -moz-border-radius: $size;
  border-radius: $size;
}
.blue{
  width: 100px;
  height: 100px;
  @include rounded-radius(10px);
}

//默认混合器传参
@mixin rounded-radius($size:20px){
  -moz-border-radius: $size;
  border-radius: $size;
}
~~~

### sass继承

@extend

~~~javascript
$blue:blue !default;
@mixin rounded-radius($size:20px){
  -moz-border-radius: $size;
  border-radius: $size;
}
.blue{
  width: 100px;
  height: 100px;
  background-color: $blue;
  @include rounded-radius(10px);
  &:hover{
    background-color: #e0e044;
  }
  h1,h2,h3{
    font-size: 16px;
  }
}
.serious{ //serious继承.blue并继承所有样式以及与.blue有关的表达式
  @extend .blue
}

~~~

+ 继承中的%

~~~javascript
//%后表示一个占位符
div%long{
  width: 100px;
  height: 100px;
}
.blue{
    @extend %long
}

//转为css
div.blue{
  width: 100px;
  height: 100px;
}
~~~

+ @media里的样式类不能继承@media外部的样式

### sass函数

+ 字符串函数
+ 数字函数
+ 列表函数
+ 三元函数
+ 颜色函数等
+ 自定义函数

~~~javascript
@function 函数名(参数){
    
}
~~~





+ 条件语句

@if(条件表达式){

}@else{

}

+ 循环语句

~~~javascript
//循环语句
@for $i from 1 to 10{ //>=1 and <10
    .border-#{$i}{
    border:#{$i}px solid blue;
	}
}

//while循环
$i:6
@while $i>0{

}
//each循环
$menber:a
each $menber in a,b,c,d{
    .#{$menber}{
    	background-image:url("/images/#${menber}.jpg")
	}
}
~~~



