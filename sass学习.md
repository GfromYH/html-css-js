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

