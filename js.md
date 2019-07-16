### 防抖



什么是防抖？

防抖就是在一些高频事件出发的时候，只触发最后一次，例如客户疯狂点击按钮，就会触发按钮事件，这个时候我们就需要用防抖来使最后一次点击生效s



代码：

~~~javascript
function debounce(fn){
    let timer=null
    return function(){
        //进入函数前先清除定时器
        clearTimeout(timer)
        timer= setTimeout(()=>{
            fn.call(this,arguments) 
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
           fn.call(this,arguments)
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
    for(let i=1,len=arguments.length;i<len;i++){
        args.push(arguments[i])
    }
    let result=context.fn(...args)
    delete context.fn;
    return result
    
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
    }else{
        let args=[]
        for(let i=0;i<arr.length;i++){
            args.push(arr[i])
        }
        result=context.fn(...args)
    }
    
    delete context.fn;
    return result
    
}
~~~



### 重写bind方法

bind返回的是一个函数



~~~javascript
Function.prototype.bind2=function(context){
    let self=this;
    //复制bind第二个到最后的参数
    let args=Array.prototype.slice.call(arguments,1);
    let fbound=function(){
        //获取bind后返回的参数
        let bindArgs=Array.prototype.slice.call(arguments)
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

还有一种比较快速的

~~~javascript
JSON.parse(JSON.stringfy(obj))
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
+ Set方法也能去重



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
        if(arguments.length===0){
            return sum
        }
        sum+=numb
        return typeofArgs
    }
}
console.log(add(1)(3)(3)());
~~~

### http状态码
~~~
200 请求成功
204 请求成功，但无法返回资源
206 对资源进行范围搜索
======================
301 永久重定向，资源转向新的url地址
302 临时重定向，资源临时转到url，对应的url可能会被修改
303 表示资源在另一个url地址上需要get方法定向获取
304 附带条件的请求（请求报文）但不满足条件请求
=========================
400 服务器无法理解请求
401 需要认证
403 不允许访问资源，存在授权问题
404 服务器无法找到该资源
405 服务器禁止使用该方法
408 请求超时
===========================
500 服务器端执行请求出现bug
502 服务器端出错往往是网关问题

~~~



### http与https与ssl加密

SSL协议是通过非对称密钥机制(非对称加密)保证双方身份认证，并完成建立连接，在实际数据通信时通过对称密钥机制(对称加密）保障数据安全性



> 非对称加密：
>
> 加密一方找到接收方的公钥e(如何找到呢？大部分的公钥查找工作实际上都是通过数字证书来实现的)，然后用公钥e对明文p进行加密后得到密文c，并将得到的密文发送给接收方，接收方收到密文后，用自己保留的私钥d进行解密，得到明文p，需要注意的是：用公钥加密的密文，只有拥有私钥的一方才能解密，这样就可以解决加密的各方可以统一使用一个公钥即可。
>
> 常用的非对称加密算法有：RSA
>
> 非对称加密的优点是：
>
> 1.不存在密钥分发的问题，解码方可以自己生成密钥对，一个做私钥存起来，另外一个作为公钥进行发布。
>
> 2.解决了密钥管理的复杂度问题，

> 对称加密：
>
> 对称加解密的过程如下：
>
> 发送端和接收端首先要共享相同的密钥k(即通信前双方都需要知道对应的密钥)才能进行通信。发送端用共享密钥k对明文p进行加密，得到密文c，并将得到的密文发送给接收端，接收端收到密文后，并用其相同的共享密钥k对密文进行解密，得出明文p。
>
> 一般加密和解密的算法是公开的，需要保持隐秘的是密钥k，流行的对称加密算法有：DES，Triple-DES，RC2和RC4
>
> 对称加密的不足主要有两点：
>
> 1，发送方和接收方首先需要共享相同的密钥，即存在密钥k的分发问题，如何安全的把共享密钥在双方进行分享，这本身也是一个如何安全通信的问题，一种方法是提前双方约定好，不通过具体的通信进行协商，避免被监听和截获。另外一种方式，将是下面我们介绍的通过非对称加密信道进行对称密码的分发和共享，即混合加密系统。
>
> 2，密钥管理的复杂度问题。由于对称加密的密钥是一对一的使用方式，若一方要跟n方通信，则需要维护n对密钥。
>
> 对称加密的好处是：
>
> 加密和解密的速度要比非对称加密快很多，因此常用非对称加密建立的安全信道进行共享密钥的分享，完成后，具体的加解密则使用对称加密。即混合加密系统。
>
> 另外一个点需要重点说明的是，密钥k的长度对解密破解的难度有很重大的影响，k的长度越长，对应的密码空间就越大，遭到暴力破解或者词典破解的难度就更大，就更加安全。
>
> ---------------------
> 作者：zxcodestudy 
> 来源：CSDN 
> 原文：https://blog.csdn.net/qq_16681169/article/details/50933958 
> 版权声明：本文为博主原创文章，转载请附上博文链接！