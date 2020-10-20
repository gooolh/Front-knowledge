## JS数据类型

### 基本类型

1. number
2. string
3. boolean
4. undefined
5. null
6. symbol

### 引用类型

1. object

## 严格模式

1. 为了消除JavaScript语法的不合理、不严谨之处
2. 为未来新版的javaScript做好铺垫

### 特性

1. 重名问题：严格模式下函数不能有重名的参数
2. this指向问题：全局作用域的函数中this不再指向全局而是undefined
3. 静态绑定：禁止使用with
4. 保留字：使用未来保留字,比如implements, interface, let, package, private, protected, public, static,和yield作为变量名或函数名会报错。
5. 不能删除变量

## `underfined`与`null`的区别

- null表示一个 `无` 的对象 underfined 表示变量未赋值
- 在转换为数字时结果不同，`Number(null)`为`0`，而``Number(undefined)`为`NaN`。

## `typeof`与`instanceof`的区别

- typeof 返回一个变量的类型，注意：基本类型除`null`返回是object 外，其他正常返回对应的数据类型，引用类型除了函数会显示为`'function'`，其它都显示为`object`。
- instanceof 判断某个构造函数是否在对象的原型链之中

## 手写一个`instanceof`

```javascript
function myInstanceof(left, right) {
  let proto = Object.getPrototypeOf(left)
  while (proto) {
    if (proto === right.prototype) return true
    proto = Object.getPrototypeOf(proto)
  }
  return false
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

## 手写call

```javascript
function myCall(target, ...args) {
  const fn = Symbol('fn')
  target[fn] = this
  const res = target[fn](...args)
  delete target[fn]
  return res
}
```

## 手写apply

```javascript
function myApply(target, args) {
  const fn = Symbol('fn')
  target[fn] = this
  const res = target[fn](...args)
  delete target[fn]
  return res
}
```

## 手写bind

```javascript
function myBind(target, ...args) {
  let _this = this
  return function () {
    return _this.apply(target, [...args, ...arguments])
  }
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
function myPromise(executor) {
  this.status = 'pending'
  this.value = ''
  this.onResolveCallback = []
  this.onRejectCallback = []
  let _this = this
  function resolve(value) {
    console.log(_this.onResolveCallback)
    setTimeout(() => {
      _this.status = 'fulfilled'
      _this.value = value
      _this.onResolveCallback.forEach((t) => t(value))
    })
  }
  function reject(reason) {
    setTimeout(() => {
      _this.status = 'reject'
      _this.value = reason
      _this.onRejectCallback.forEach((t) => t(reason))
    })
  }
  try {
    executor(resolve, reject)
  } catch (error) {
    reject(reject(error))
  }
}
myPromise.prototype.then = function (onFulfilled, onRejected) {
  return new myPromise((resolve, reject) => {
    this.onResolveCallback.push((value) => {
      try {
        const res = onFulfilled(value)
        if (res instanceof myPromise) {
          res.then(resolve, reject)
        } else {
          resolve(res)
        }
      } catch (error) {
        reject(error)
      }
    })
    this.onRejectCallback.push((value) => {
      try {
        const res = onRejected(value)
        if (res instanceof myPromise) {
          res.then(resolve, reject)
        } else {
          reject(res)
        }
      } catch (error) {
        reject(error)
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


## 隐式转换规则

 转换为字符串规则

```javascript
数组：相当于执行join(',') 数组中的null和undefinded 转为空字符串  空数组转为空字符串 
对象：直接调用Object.prototype.toString(),返回'[Object,Object]'
其他类型：加个双引号 如'null'
```

转换为数字

```
null： 转为0
undefined：转为NaN
字符串：如果是纯数字形式，则转为对应的数字，空字符转为0, 否则一律按转换失败处理，转为NaN
布尔型：true和false被转为1和0
数组：数组首先会被转为原始类型，也就是ToPrimitive，然后在根据转换后的原始类型按照上面的规则处理，关于ToPrimitive，会在下文中讲到
对象：同数组的处理
```

转为布尔类型

```
js中的假值只有false、null、undefined、空字符、0和NaN，其它值转为布尔型都为true。 
注意没有'0'
```

toPromitve

`ToPrimitive`指对象类型类型（如：对象、数组）转换为原始类型的操作。

```
当对象类型需要被转为原始类型时，它会先查找对象的valueOf方法，如果valueOf方法返回原始类型的值，则ToPrimitive的结果就是这个值
如果valueOf不存在或者valueOf方法返回的不是原始类型的值，就会尝试调用对象的toString方法，也就是会遵循对象的ToString规则，然后使用toString的返回值作为ToPrimitive的结果。
```



## web worker

Javascript是单线程语言，意味着同一时间只执行一个任务，无法发挥多核cpu的优势，**web worker**的作用是为Javascript创建多线程环境，允许主线程创建Worker线程，我们可以将一些复杂运算分配worker执行，等worker线程执行完，再将结果返回给主线程。这样的好处是主线程不会被复杂运算阻塞，就会很流畅。

### 注意事项：

1. 同源限制
   分配给worker线程运行的脚本文件，必须和主线线程的脚本文件同源

2. DOM限制

   worker线程是无法读取document、window、parent这些对象，所以无法操作DOM元素

3. 通信联系
   worler线程和主线程不在同一上下文环境中，它们是不能直接通信的，需要通过消息传递完成

4. 脚本限制
   worker线程不能执行alert()和confirm()

5. 文件限制
   无法读取本地文件

### 基本使用

```javascript
var worker=new Worker('work.js') //创建一个worker线程
worker.postMessage('hello')  //主线程向worker发送消息
worker.onmessage=function(e){  //主线程接收worker发送过来的消息
	// ... 
}
worker.terminate() //结束worker线程

// worker内部
self.addEventListener('message',function(e){  //监听主线程发送过来的消息
   // ...
})
importScripts('xxx.js') //加载其他js文件
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

## JavaScript 原型，原型链？ 有什么特点？

```
在js中我们是通过构造函数来新建一个对象的，每个构造函数内部都会有一个原型对象 prototype的属性，这个对象含有所有实例共享的属性和方法，通过构造函数新建一个对象后，在这个对象的内部含有一个指针__proto__ 隐式原型，指向构造函数的原型对象，
原型链：当我们访问一个对象的属性时，如果在对象的内部不存在这个属性，那么他会去原型对象里找，这个原型对象自己又会有自己的原型，于是就这样一直找下去，就是原型链的概念，原型链的终点时Object.prototype
```



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

## JavaScript中的执行上下文和执行栈

### 执行上下文

​	简单来说，执行上下文是js代码的执行环境，它有三种类型

1. **全局执行上下文**	它是一个默认的执行的上下文，不在函数中的代码都会在全局的上下文中执行
2. **函数执行上下文**   每次调用函数，都会创建一个函数上下文
3. **Eval函数执行上下文**

### 执行栈

​	用来存储代码运行中创建的执行上下文，一开始它会创建一个全局的执行上下文然后压到执行栈中，每当遇到一个函数执行，都会创建一个函数上下文并压进栈中，当函数执行结束后会弹出来。

### 执行上下文的创建

创建阶段包含

- **创建词法环境**
- **创建变量环境**

### 创建词法环境

词法环境简单来说是，变量和变量名的映射，每个词法环境由3个部分组成

1. **环境记录** 存放着变量和函数的声明
2. **外部环境的引用**   意味着可以找到外部执行执行上下文，也就是说如果在自己执行上下文中找不到变量，会去找外部执行上下文
3. **绑定`this`**    谁调用函数，函数的`this`就会绑定这个对象，如果没有的话就会执行`window`,严格模式会指向`undefined`

### 创建变量环境

​	**词法环境**和**变量环境**最大的区别是前者存储着函数声明和使用（let/const）的变量，后者只存储var声明的变量

在创建阶段，使用`let/const`定义的变量没有被赋值，但使用`var`声明的变量会被赋值`undefined`,这就是**变量声明提升**

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
- 高频点击提交，表单重复提交
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



## mouseenter、mouserleave 和mouseover、mouseout的区别

mouseover：当鼠标移入元素或其子元素都会触发事件，所以有一个重复触发，冒泡过程。对应的移除事件是mouseout

mouseenter:当鼠标移除元素本身（不包含元素的子元素）会触发事件，也就是不会冒泡，对应的移除事件是mouseleave

mouseover 事件因其有冒泡事件，在子元素内移动，频繁被触发，如果我们不希望这样,可以使用mouseenter代替

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

## 性能优化

1. 减少资源请求数量
   合并静态资源，减少重定向，使用缓冲，避免使用空a标签和form表单设置空method

2. 减少资源大小
   css压缩：删除无效代码，合并语义，js压缩：删除注释，代码缩减，减低代码可读性，
   服务端使用gzip

3. 优化网络连接
   使用CDN 内容分发网络，可以根据网络流量和个节点的连接，负载状况以及用户的距离将连接导向用户最近的服务节点上，提高网址的响应速度
   使用DNS预解析

4. 优化资源加载
   加载位置：css放在head js放在body底部

   模块按需加载：在SPA等业务复杂的系统中，按需加载

5. 减少回流重绘

   不要使用table布局，避免频繁读取，offset

   动画脱离文档流

   防抖和节流

6. 使用性能更好的API

   使用IntersectionObserver来实现图片的懒加载：传统的做法监听scroll事件加防抖，并调用getBoundingClientRect
   使用web worker 处理大计算量的代码，避免阻塞用户交互

