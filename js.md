### 防抖



什么是防抖？

防抖就是在一些高频事件出发的时候，只触发最后一次，例如客户疯狂点击按钮，就会触发按钮事件，这个时候我们就需要用防抖来使最后一次点击生效s



代码：

~~~javascript
function debounce(fn){
    let timer=null
    return function(){
        //进入函数前先清除定时器
        clearTimeOut(timer)
        timer= setTimeout(()=>{
            fn.call(this,arguements)
            
        },1000)
    }
}
~~~



### 节流

什么是节流？

节流就是当你第一次点击时，会立即触发，在接下来一段时间内，点击都不会触发，直至这段时间过后才会再次触发 用处：例如图片懒加载，需要 计算滚动条的高度



代码

~~~javascript
function throttle(fn){
   let canrun=true
   return function(){
       //避免重复触发
       if(!canrun){
           return;
       }
       //避免重复触发
       canrun=false
       setTimeout(()=>{
           fn.call(this,arguements)
           canrun=true
       },1000
   }
}
~~~



### 重写call方法

call主要就三步

1. 改变this指向
2. 在新对象里调用该方法
3. 删除新增的方法

主要参考牙羽的blog 我fork了

~~~javascript
Function.prototype.call2=function(context){
    //当传入的参数是null时
    let context=context||window
    context.fn=this
    let args=[]
    for(let i=1,len=arguements.length;i<len;i++){
        args.push(arguements[i])
    }
    let result=context.fn(...args)
    r
    
}
~~~



### 重写apply方法

apply与call类似只不过传入的数组，而不是像call一样传入的参数一个一个写在后面



~~~javascript
Function.prototype.apply2=function(context,arr){
    let context=Object(context)||window;
    context.fn=this
    let result
    if(!arr){
        result = context.fn()
    }
    let args=[]
    for(let i=0;i<arr.length;i++){
        args.push(arr[i])
    }
    let result=context.fn(...args)
    return result
    
}
~~~



### 重写bind方法

bind返回的是一个函数



~~~javascript
Function.prototype.bind2=function(context){
    let self=this;
    //复制bind第二个到最后的参数
    let args=Array.prototype.slice.call(arguements,1);
    let fbound=function(){
        //获取bind后返回的参数
        let bindArgs=Array.prototype.slice.call(arguements)
        //如果bind返回的是构造函数 那么根据下面改变它的原型指向 则返回true
        // 当作为普通函数时，this 指向 window，self 指向绑定函数，此时结果为 false，当结果为 false 的时候，this 指向绑定的 context。
        self.apply(this instanceof self ? this : context, args.concat(bindArgs));
    }
    fbound.prototype=this.prototype
    return fbound
}
~~~

