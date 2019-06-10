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



### 重写new



1. 创建一个空对象
2. 将空对象的原型指向与目标构造函数的原型



~~~javascript
function objectFactory(){
        let obj={}
        //取出第一个参数也就是构造函数
         Constructor=[].shift.call(arguments)
        //将空对象的原型指向与目标构造函数的原型
        obj.__proto__=Constructor.prototype
        //改变this指向，并付给参数，由于arguments被shift一个参数所以剩下的即使需要的
        let ret=Constructor.apply(obj,arguments)
        return typeof ret==='object'? ret:obj //判断返回的始终是一个object对象
}
~~~



### 浅拷贝与深拷贝

浅拷贝与深拷贝的区别，简单来说A拷贝了B，A中的值变化了，B也跟着变化，这叫浅拷贝，深拷贝则是A中变化了，B的值不改变。也可以将是浅拷贝+递归实现深拷贝



+ 浅拷贝

~~~javascript
function lowClone(obj){
    if(typeof obj!=='object'){
        return
    }
    let newObj=obj instanceof Array?[]:{}
    for(key in obj){
        if(obj.hasOwnProperty(key))
            newObj[key]=obj[key]
    }
    return newObj
}
~~~



+ 深拷贝

~~~javascript
function deepClone(obj){
    if(typeof obj!=='object'){
        return
    }
    let newObj=obj instanceof Array?[]:{}
    for(key in obj){
        if(obj.hasOwnProperty(key))
            newObj[key]=typeof obj[key] ==='object' ?deepClone(obj[key]):obj[key]
    }
    return newObj
}
~~~



### 去重

我觉得这个可能是常考到的一种类型



昨天翻阅了MDN发现reduce也能进行去重

这里就摘录一下reduce去重的方法

+ reduce

~~~javascript
let arr=[1,2,3,4,5,2,1,4,21,4,233,1,2,1]
let newArr=arr.sort((a,b)=>a-b).reduce((acc,cur)=>{
    if(acc.length===0||acc[acc.length-1]!==cur){
        acc.push(cur)
    }
},[])
console.log(newArr) 就能去重了

~~~



+ filter

~~~javascript
let arr=[1,2,3,4,5,2,1,4,21,4,233,1,2,1]
let newArr=arr.filter((ele,index,self)=>{
    return self.indexOf(ele)===index
})
~~~

+ indexOf

~~~javascript
    function unique(array) {
        let res=[]
        for (let i = 0,arrLen=array.length; i <arrLen ; i++) {
            if (res.indexOf(array[i]) === -1) {
                res.push(array[i])
            }
        }
        return res
    }
~~~

+ 原始方法

~~~javascript
 function unique(array) {
        var res = [];
        for (var i = 0, arrayLen = array.length; i < arrayLen; i++) {
            for (var j = 0, resLen = res.length; j < resLen; j++ ) {
                if (array[i] === res[j]) {
                    break;
                }

            }
            console.log("j=" + j)
            if (j === resLen) {
                res.push(array[i])
            }
        }
        return res;
    }

~~~



### 扁平化

也是利用递归，拆分数组

~~~javascript
    function flatArr(array) {
        let result=[]
        for (let i = 0,len=array.length; i <len ; i++) {
            if (Array.isArray(array[i])){
                result=result.concat(flatArr(array[i]))
            }
            result.push(array[i])
        }
        return result
    }
    let array=[1,[2,[3,4]]]
    console.log(flatArr(array));
~~~



### 柯里化

~~~javascript
function add(num){
    let sum=0;
    sum+=num
    return function typeofArgs(numb){
        if(arguements.length===0){
            return sum
        }
        sum+=numb
        return typeofArgs
    }
}
console.log(add(1)(3)(3)());
~~~

