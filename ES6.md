

## let和const 命令

```
let 用于申明变量,用法类似与 var ，但是声明的变量只在let 所在的代码块生效
使用var会出现变量提升 如果在申明前使用 值为undefined  使用let会 报错 not defined
暂时性死区：const 和let 声明变量时会与当前作用域绑定 在声明之前不能使用其变量 如果外部也存在同名的变量也不可以 因为变量已经与当前作用域绑定了

```

## 变量的解构赋值

```
按照一定的模式 从数组和对象中提取值,给变量赋值 称之为解构
let [a,b,c]=[1,2,3] 
如果等号右边不是数组 或者认为没有具有iterator接口 就不可以解构
let [foo]=1
let [foo]=false
具有iterator接口
let [a,b,c]=new Set(['a,b,c'])
funcion *fibs(){
	let a=0
	let b=1
	while(true){
		yield a
		[a,b]=[b,a+b]
	}
}
ll
let [first, second, third, fourth, fifth, sixth] = fibs();
sixth // 5
Generator 具有Iterator 接口，解构赋值会依次从这个接口赋值
对象的解构
对象的解构与数组的解构不同，数组是根据元素索引来决定的,而对象是根据属性名来决定的
let {a,b}={a:'hello',b:'hi'}
// a='hello'
// b='hi'
如果变量名与属性名不同 ,也就是说先找到同名的属性，然后赋值给后面的变量
let {a:b}={a:'aaaa',b:'bbbb'}
// b='aaaa'

```

## 函数的扩展

```
参数的默认值 惰性求值(每次调用都会计算) 函数默认值一般设置在参数末尾,如果设置在前面则不可以省略 注意传入null时不会使用默认值。
与解构赋值默认值结合使用
function foo({x,y=5}){
	console.log(x,y)
}

foo({}) //x=undefined
foo() //报错
只有参数为对象时参数才会解构赋值生成。否则出现 x is undefined

rest 参数
ES6引入了rest参数(...参数)，rest参数之后不能有其他参数,这样子就不用使用arguments对象了，rest参数是一个数组，把多余的参数放在数组里面。
function add(...nums){
	var sum=0
	for(let n of nums){
		sum+=m
	}
	return sum
}
const sortNums=(...nums)=>nums.sort()

箭头函数 
使用（=>）定义函数
var f= v=> v
相当于
function f(v){
	return v
}
如果箭头函数不需要参数或需要多个参数 使用一个圆括号代表参数部分
var f=()=>5
相当于
function f(){ return 5 }
var f=(num1,num2)=> num+num2
相当于
function f(num1,num2){ return num1+num2 }
如果箭头函数代码块多于1行 则用括号包裹起来
注意事项 
箭头函数不能用于构造函数
箭头函数没有this
不可使用 yield命令,因此不能用作Generator函数

```



## 数组的扩展

扩展运算符是由3个点（...）组成

```
将数组转成以逗号分隔的参数序列
console.log(...[a,b,c])
//a b c
function add(x,y){
	return x+y;
}
add(...[1,3]) //4

合并数组
const arr=[a,b,...[c,d]]

与解构赋值一起使用 注意参数要放在最后一位 否则会报错
const [a,...rest]=[1,2,3,4,5]
//a=1,rest=[2,3,4,5]
没具有iterator接口使用 会报错
const a={
	a:1,
	b:2
}
let arr=[...a]  //报错
扩展运算符内部调用的是iterator 接口，因此只要具有iterator接口的对象，都可以使用扩展运算符
例如 set和map 
const a=new Set([1,2,3,4])

```

## 对象的扩展

```
简洁的写法 可以用变量的名字当坐键值，这样的写法方面函数的返回值
const foo="hello"
const obj={foo}
// obj={foo}
function fun(){
	const x=1
	const y=2
	return {x,y}
}
//{x:1,y:2}
属性名表达式
const name='iam'
const a={
	[name]:123
}
注意不能和简洁表达式一起写
const name='iam'
const a={[name]} //报错
注意如果属性名为对象的话 转成字符变成[object object]

使用扩展运算符
const obj={
	a:1,
	b:2
}
const obj={...obj}

```

## Iterator 和for...of 循环

Iterator(遍历器)：它是一个接口，为各种不同的数据结构提供统一的访问机制。 任何数据结构只要部署Iterator接口， 就可以完成遍历操作（ 即依次处理该数据结构的所有成员） 。ES6创造了一种新的遍历命令for...of循环，Iterator接口主要供for...of消费。

Iterator的遍历过程：

1. 创建一个指针对象，指向当前数据结构的起始位置，遍历器对象本质上，就是一个指针。
2. 第一次调用指针对象的 next 方法， 可以将指针指向数据结构的第一个成员。  
3. 不断调用指针对象的 next 方法， 直到它指向数据结构的结束位置。  

每一次调用 next 方法， 都会返回数据结构的当前成员的信息。 具体来说， 就是返回一个包含 value 和 done 两个属性的对象。 其中， value 属性是当前成员的值， done 属性是一个布尔值， 表示遍历是否结束。  

默认的Iterator接口

ES6规定，默认的Iterator接口部署到数据结构中的Symbol.iterator属性，或者说，一个数据接口只要具有Symbol.iterator属性，就可以认为是可遍历的。

原生支持Iterator接口的数据结构：

1. Array
2. Map
3. Set
4. String
5. 函数的 arguments 对象  
6. TypedArray  
7. NodeList对象



## Promise

Promise是异步编程解决方案，比起传统的解决方案-回调函数和时间-更加合理和强大。对象的状态不受外界影响。Promise对象代表一个异步操作。
Promise有3种状态  pending(进行种)  fulfilled(已成功)和rejected(已成功)。
状态一旦改变，就不会再变，Promise对象的状态改变，只有2种可能：从pending变成fulfilled和从pending变成rejected。

```

//基本使用
const promise=new Promise((resolve,reject)=>{
	if(success){
		resolve(data)
	}else{
		reject(err)
	}
})
Promise的构造函数接收两个参数，一个是resolve,rejcet 。resolve从pending 状态变为成功的状态，reject从pending状态变为失败的状态。

Promise.prototype.then方法接收2个回调函数作为参数。第一个是resolve调用的，第二个是reject调用的，是可选的。该方法返回一个新的Promise实例，如果返回值不是promise,会调用promise.resolve方法转为promise.因此可以采用链式写法。

Promise.prototype.catch 方法是.then(null,rejection)的别名，用于指定发送错误时的回调函数。Promise 对象的错误具有“冒泡”性质， 会一直向后传递， 直到被捕获为止。 也就是
说， 错误总是会被下一个 catch 语句捕获。如果在promise里面有错误没有捕获，一般是不会外传。

Promise.all()
Promise.all 方法用于将多个 Promise 实例， 包装成一个新的 Promise 实例。
只有当全部promise实例都变为fufilled成功状态时，才会返回成功状态，有点像`与`操作
Promise.all([p1,p2]) 传入的参数都是Promise，如果不是会先调用Promise.resolve方法

Promise.race()
与Promise.all()差不多，race有种竞速的意思，看谁率先完成，先完成的会根据它的状态该表。如设置一个超时时间，如果请求超过这个时间，就返回失败的状态。

Promise.resolve()
有时需要将现有对象转为Promise对象， Promise.resolve 方法就起到这个作用。
使用时分为4种情况
1：参数为promise 直接返回
2：参数为thenable对象（对象含有then的方法）先转为Promise 在执行then方法
3：参数是个值,转为Promise再执行resolve
4：无参，直接返回一个 resolved 状态的Promise对象。

Promise.reject()
Promise.reject(reason) 方法也会返回一个新的 Promise 实例， 该实例的状态为 rejected

done()
Promise对象的回调链， 不管以 then 方法或 catch 方法结尾， 要是最后一个方法抛出错误， 都有可能无法（因为Promise内部的错误不会冒泡到全局） 。因此， 我们可以提供一个 done 方法， 总是处于回调链的尾端保抛出任何可能出现的错误。done 都会捕捉到任何可能出现的错误， 并向全局抛出。

finally()
finally 方法用于指定不管Promise对象最后状态如何， 都会执行的操作。 它与 done 方法的最大区别， 它接受普通的回调函数作为参数， 该函数不管怎样都必须执行

Promise.try() 
如果想不是异步的操作在Promise中同步执行，
统一的处理错误异常，
使用Promise 库Bluebird、Q等，或引入Polyfill

20行实现Promise 参考网上的

```

## Generator

Generator 是ES6提高的一种异步编程的解决方案。Generator不用于普通函数，在函数名前加个星号，函数内部可使用yield表达式，执行Generator函数会返回一个遍历器对象，代表Generator的内部指针，每次调用遍历器对象的next方法，就会返回含有value和done属性的对象，value代表内部状态的值，yield表达式后面的值，done是个bool类型，表示是否结束。

```
function *f(){
	yield 1
	return 2
}
const g=f() //返回一个遍历器对象
g.next() //{value:1,done:false}
g.next() //{value:2,done:true}
g.next() //{value:undefined,done:true}
```

