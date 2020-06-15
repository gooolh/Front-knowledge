## JS基本数据类型

1. number
2. string
3. boolean
4. undefined
5. null
6. symbol

## `underfined`与`null`的区别

- null表示一个 `无` 的对象 underfined 表示变量未赋值
- 在转换为数字时结果不同，`Number(null)`为`0`，而`undefined`为`NaN`。

## `typeof`与`instanceof`的区别

- typeof 返回一个变量的类型，注意：基本类型除`null`返回是object 外，其他正常返回对应的数据类型，引用类型除了函数会显示为`'function'`，其它都显示为`object`。
- instanceof 判断某个构造函数是否在对象的原型链之中

## 手写一个`instanceof`

```javascript
function myInstanceof(left,right){
    let proto=Object.getPrototypeOf(left)
    while(true){
        if(proto===null) return false;
        if(proto===right) return true;
        proto = Object.getPrototypeOf(proto)
    }
}
```

## 手写一个`new`

```javascript
function myNew(fn,...args){
    let instance =Object.create(fn.prototype)  //根据fn的原型创建对象
    let result=fn.call(instance,...args)	//通过构造函数初始化对象，
    return result==='object'? result:instance   //如果构造返回一个对象的话，就使用这个对象，否则使用刚才创建的实例
}
```



## 手写AJAX

```javascript
function ajax(options={}) {
    options=Object.assign({
        url:'',
        method:'get',
        timeout: 1000,
        data: null,
        header:{},
        success:function () {

        },
        onerror:function () {

        }
    },options)

    let xhr=new XMLHttpRequest()
    xhr.timeout=options.timeout
    xhr.setRequestHeader(options.header)
    xhr.open(options.method,options.url)
    xhr.onreadystatechange=function () {
        if (xhr.readyState==4 && xhr.status==200){
            let data=JSON.parse(xhr.response)
            options.success(data)
        }else {
            options.onerror()
        }
    }
    xhr.ontimeout=options.onerror()
    xhr.send(options.data)
}
```

## 手写Promise

```javascript
//简版Promise
//executor参数是一个方法，方法有个两个参数，resolve，reject 一样为函数，这样简版就没有实现reject
function Promise(executor) {
    //用一个数组保存then回调的事件
    this.onResolveCallback = []
    let _this=this
    function resolve(value) {
        setTimeout(() => {
            _this.data = value
            _this.onResolveCallback.forEach(item => item(value))
        })
    }
    executor(resolve)
}
Promise.prototype.then = function (onResolved) {
    const _this = this
    return new Promise(resolve => {
        _this.onResolveCallback.push(value => {
            const result = onResolved(value)
            //交个用户自定义Promise处理，使用then方法添加事件，再回调处理。
            if (result instanceof Promise) {
                result.then(resolve)
            } else {
                //执行回调
                resolve(result)
            }
        })
    })
}
```



## 浅拷贝

浅拷贝是只复制某个对象的指针，而不是复制对象本身，新老还是共享同一块内存。

```javascript
let obj={
	name:'gooolh',
	son:{
		sham:'1'
	}
}

let obj2=obj
//与上面一样
let obj3=Object.assign(obj2)
//只深拷贝了一层，修改son还是会影响其他变量
let obj4=Object.assign({},obj2)
```



## 深拷贝

深拷贝会另外创造一个一模一样的对象，新老对象不共享内存，修改新对象不会影响到老对象。

实现方法

- ```javascript
  //递归拷贝
  function deepClone(obj) {
      let objClone=Array.isArray(obj)?[]:{}
      for (const key in obj) {
          if (typeof obj[key]==='object'){
              objClone[key]=deepClone(obj[key])
          }else {
              objClone[key]=obj[key]
          }
      }
      return objClone
  }
  ```
  
- ```
  //常用的方法，如果对象是function则会发生错误
  JSON.parse(JSON.stringfiy());
  ```

- ```
  //如果对象只有一层，可以直接使用Object.assign()
  let obj=Object.assign({},old) //注意第一个对象要传个对象，不传的话会直接返回老对象。
  ```

  

## JS事件执行机制

### 前言

先了解一下进程与线程的区别，简单来说 进程是cpu资源分配的最小单位，线程是cpu调度的最小执行单位，一个进程有一个或多个进程，线程之间共享着进程的空间,而进程与进程不独立的一块内存。

js是单线程的，这样设计是因为js作为浏览器的脚本语言，主要用途是与用户交互，而用户的交互为非就是对DOM增删改。如果js有多个线程，操作dom的时候，因为不清楚以哪个结果为准，会产生混乱。后来html5 引入了web worker, 允许 JavaScript 脚本创建多个线程，但是子线程完全受主线程控制，且不得操作 DOM。所以，这个新标准并没有改变 JavaScript 单线程的本质。

javaScript是单线程，怎么处理异步操作呢?  

浏览器有着几个线程：

1. GUI线程，即dom渲染线程，用于绘制dom树
2. js线程，执行js脚本
3.  事件触发线程
4. 定时器线程
5. 网络请求线程

GUI线程和js线程是互斥的，会相互阻塞。

单线程意味着要排队，一次只能执行一个任务。如果有多个任务，就需要排队一个一个执行。

js引擎使用事件循环（event loop）机制来协调网络请求，UI交互，事件，脚本，UI渲染等行为。当执行完一次事件循环后。也就是说执行栈为空时，会轮询消息队列取出任务来执行。

宏任务 （Macro Tas） 和 Micro Task（微任务）

微任务和宏任务皆为异步任务，分别有一个任务队列，主要区别在于他们的执行顺序，Event Loop的走向和取值。

每次执行栈执行的代码就是一个宏任务，包括任务队列(宏任务队列)中的，因为执行栈中的宏任务执行完会去取任务队列（宏任务队列）中的任务加入执行栈中，即同样是事件循环的机制。

宏任务包括  主代码块 setTimeout 、setInterval 、setImmediate 、I/O 、UI rendering

微任务主要包含：Promise.then、MutaionObserver 、process.nextTick

来个例子吧

```javascript
console.log(1)
setTimeout(() => {
    console.log(5)
    Promise.resolve().then(() => {
        console.log(6)
    })
})
Promise.resolve().then(() => {
    console.log(3)
})
Promise.resolve().then(() => {
    console.log(4)
})
setTimeout(() => {
    console.log(7)
})
console.log(2)
// 按 1到7打印
```

[参考文档](https://segmentfault.com/a/1190000013083967#item-5) 

### 总结

js它是单线程的，所以执行任务时需要排队，JS异步执行机制是事件循环，遇到异步操作的时候，并不需要等待执行完成，而是先注册到事件表，交给对应处理浏览器对应的线程处理，所有的同步任务在该主线程中执行，遇到异步任务，对应任务添加到事件线程中，等对应事件准备好了，就放到任务队列中，宏任务放到宏任务队列，微任务放到微任务队列，然后通过轮询消息队列，先执行宏任务，然后执行所有的微任务，如果宏任务产生了微任务，则会在下一次事件循环执行，更新 render,主线程重复执行上述步骤。

## 描述一下作用域链

(答案参考：[《面试分享：两年工作经验成功面试阿里P6总结》](https://juejin.im/post/5d690c726fb9a06b155dd40d#heading-125))

当代码在一个环境中创建时，会创建变量对象的一个作用域链（scope chain）来保证对执行环境有权访问的变量和函数。作用域第一个对象始终是当前执行代码所在环境的变量对象（VO）。如果是函数执行阶段，那么将其activation object（AO）作为作用域链第一个对象，第二个对象是上级函数的执行上下文AO，下一个对象依次类推。

在《JavaScript深入之变量对象》中讲到，当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。

## 闭包

闭包：函数和函数所在的环境

特性：

- 函数嵌套函数
- 内部函数使用外部函数的参数和变量
- 参数和变量不会被垃圾回收机制和回收

缺点：

- 常驻内存，增加内存使用量
- 使用不当会造成内存泄露

使用场景：

- 函数防抖和节流

## 节流

节流是函数在规定时间内只执行一次，在规定时间内，多次执行，只有一个生效。

```javascript
function throttle(fn,interval) {
    let flag=true
    return function (...args) {
        const _this=this
        if (!flag) return;
        flag=false
        setTimeout(()=>{
            fn.apply(_this,args)
            flag=true
        },interval)
    }
```

使用的场景

1. 高频点击提交，表单重复提交
2. 监听滚动事件，比如是否滑到底部自动加载更多，用throttle来判断

## 防抖

防抖是在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。

```javascript
function debounce(fn,interval) {
    let timer=null
    return function (...args) {
        const _this=this
        if (timer) clearTimeout(timer)
        timer=setTimeout(()=>{
            fn.apply(_this,args)
        },interval)
    }
}
```

使用的场景

- 搜索框搜索输入。只需用户最后一次输入完，再发送请求
- 手机号、邮箱验证输入检测
- 窗口大小Resize。只需窗口调整完成后，计算窗口大小。防止重复渲染。

双剑合璧

因为防抖有时候触发的太频繁会导致一次响应都没有，我们希望到了固定的时间必须给用户一个响应。

```javascript
function throttle(fn,delay){
    let last=0,timer=null
    return function(...args){
        const _this=this
        let now=new date()
        if(now-last<delay){
            clearTimeott(timer)
            setTimeout(()=>{
                last=now
                fn.apply(fn,args)
            })
        }else{
            // 这个时候表示时间到了，必须给响应
    	   last = now;
      	   fn.apply(context, args);
        }
    } 
}
```



## 图片懒加载

图片只有在我们的视窗内才加载图片，可以节省带宽

### 方案一: clientHeight、scrollTop 和 offsetTop

```javascript
<img src="default.jpg" data-src="http://www.xxx.com/target.jpg" />
 
let img =document.getElementsByTagName("img")
let num=img.length
window.addEventListener('scroll',throttle(lazyLoad,200))
function lazyLoad(){
    let viewHeight=document.documentElement.clientHeight
    let scrollTop=document.documentElement.scrollTop
    for (let i = 0; i < num; i++) {
        if(img[i].offsetTop < scrollTop+viewHeight){
            if(img[i].getAttribute("src") !== "default.jpg") continue;
        	img[i].src = img[i].getAttribute("data-src");
        }
    }
}
```

### 方案二：getBoundingClientRect

```javascript
将上面的方案修改为
function lazyLoad(){
    let viewHeight=document.documentElement.clientHeight
	for (let i = 0; i < num; i++) {
         if(img[i].getBoundingClientRect.top < scrollTop+viewHeight){
            if(img[i].getAttribute("src") !== "default.jpg") continue;
        	img[i].src = img[i].getAttribute("data-src");
        }
    }
}
```

### 方案三：IntersectionObserver

这是浏览器内置的一个`API`，实现了`监听window的scroll事件`、`判断是否在视口中`以及`节流`三大功能。

```javascript
<img src="default.jpg" data-src="http://www.xxx.com/target.jpg" />

const observer=new InterSectionObserver(changes=>{
	for(let i=0;i<changes.length;i++){
        let change=changes[i]
        if(change.isIntersecting){
            const el=change.target
            el.src=el.getAttribute("data-src");
            ovserver.unobserve(change)
        }
    }
})
Array.from(img).forEach(item=>obserber(item))
```

## 实现一个批量请求函数 multiRequest(urls, maxNum)

要求：

1. 要求最大并发数 maxNum
2. 每当有一个请求返回，就留下一个空位，可以增加新的请求
3. 所有请求完成后，结果按照 urls 里面的顺序依次打出

```javascript
function handleFetchQueue(urls, max, callback) {
    const urlCount = urls.length;
    const requestsQueue = [];
    const results = [];
    let i = 0;
    const handleRequest = (url,j) => {
        const req = fetch(url).then(res => {
            results[j]=res
            console.log(results.length)
            if (i + 1 < urlCount) {
                requestsQueue.shift();
                handleRequest(urls[++i],i)
            } else if (results.length === urlCount) {
                console.log(results)
                'function' === typeof callback && callback(results)
            }
        }).catch(e => {
            results[j]=e
        });
        if (requestsQueue.push(req) < max) {
            handleRequest(urls[++i],i)
        }
    };
    handleRequest(urls[i],i)
}
```
