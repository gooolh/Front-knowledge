

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

## Set和WeakSet

- ES6提供的新的数据结构，它类似数组，但是它的成员是唯一的，没有重复的值
- WeakSet和Set类似，不过的值只能是对象，没有size属性，不能被遍历，它的对象是弱引用的，也就是说WeakSet的对象不会考虑到垃圾回收机制，即只有其他的对象引用该对象,则该对象会被回收
-  可以用来保存DOM节点，不容易造成内存泄漏

## Map和WeakMap

- Map存储键值对，和对象类似，但是它的key可以是对象，比对象有内置了许多方法,比如entires,values,keys方法，这些方法都实现了iterator接口，遍历更加方便
- WeakMap类似于Map，键必须是对象。不能被遍历，它的键值是弱引用的

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

### yield 表达式  

由于 Generator 函数返回的遍历器对象， 只有调用 `next` 方法才会遍历下一个内部状态， 所以其实提供了一种可以暂停执行的函数。 yield 表达式就是暂停标志。  

当遇到`yield` 表达式时会暂停后面的执行，返回`yield` 表达式后面的值，等下一次调用`next`方法时，会从暂停的地方开始执行.注意`yield`表达式只能出现在Generator 函数里面。

### 与`Iterable`的关系

之前说过，`Iterator`  接口也是返回一个遍历器对象，有了`Iterator`接口我们可以很轻松的遍历内部数据,比如使用`for of`和扩展运算符

有了Generator 函数我们可以很轻松在一个对象上部署`Iterator`  接口，只需要`[Symbol.iterator]=function *gen(){}` 就行了,比如

```javascript
let obj={}
obj[Symbol.iterator]=function *gen(){
    yield 1
    yield 2
}
console.log([...obj])  //[1,2]

```

### next方法的参数

`yield`表达式本身没有返回值，next方法可以带一个参数，该参数就会当作上一个`yield`的返回值了

例子

```javascript
function* dataConsumer() {
    console.log('Started');
    console.log(`1. ${yield}`);
    console.log(`2. ${yield}`);
    return 'result';
}
let genObj = dataConsumer();
genObj.next();
// Started
genObj.next('a')
// 1. a
genObj.next('b')
// 2. b
```

### Generator.prototype.throw()

Generator 原型对象上的方法，可以在Generator 抛出错误，在Generator函数体内捕获。如果函数体内没有部署`try...cathch`代码块捕获，将会被外部的`try...cathch`代码块捕获。

例子

```javascript
var g = function* () {
    try {
        yield 2;
        yield 3;
    } catch (e) {
        console.log('内部捕获', e);
    }
};
var i = g();
i.next();
try {
    i.throw('a');
    i.throw('b');
} catch (e) {
    console.log('外部捕获', e)
}
// 内部捕获 a
// 外部捕获 b
```



## Generator.prototype.return()  

Generator 函数返回的遍历器对象， 还有一个 return 方法， 可以返回给定的值，并且终结遍历 Generator 函数。  

```javascript
function* gen() {
    yield 1;
    yield 2;
    yield 3;
}
v
let g = gen();
g.next() // { value: 1, done: false }
g.return('foo') // { value: "foo", done: true }
g.next() // { value: undefined, done: true }
```



### yield* 表达式

如果函数内部调用另一个Generator函数，默认情况是没有效果的。

这时候就需要用到yield* 表达式了，

比如数组扁平化的例子

```javascript
function *gen(arr) {
    for (const item of arr) {
        if (item instanceof Array) {
            yield* gen(item)
        }else{
            yield item
        }
    }
}
```

## class的基本语法

在ES6出现之前，生成对象的方法是通过构造函数。而ES6的`class`可以看作一个语法糖，它的绝大部分功能，ES5都可以做到,新的`class`写法只是让对象的原型的写法更加清晰。

```javascript
class Point{
	constructor(x,y){
        this.x=x
        this.y=y
    }
    toString(){
        return '(' + this.x + ', ' + this.y + ')';
    }
}

typeof Point // "function"
Point === Point.prototype.constructor // true
```

类的数据类型就是函数，类本身就指向构造函数。

`class`里面的方法都定义在类的`proototype` 属性上面。等同于ES5中

```javascript
Point.prototype.toString=funtion(){}
```

但是 与ES5中不同的是,**ES6创建的不可枚举的。**

类和模块的内部， 默认就是严格模式。   

### constructor 方法

`constructor` 方法是类的默认方法，通过`new`命令生成对象实例时，自动调用该方法。一个类必须有`constructor`方法，如果没有显示定义，就会添加一个空的进去。

constructor 方法默认返回实例对象（ 即 this ） ， 完全可以指定返回另外一个对象  

### class表达式

与函数一样，类也可以使用表达式的形式定义

```javascript
const point=class{}
```

### 不存在提升

类不存在变量提升，与ES5不一样。因为子类必须在父类后面定义。

### 静态方法

类相当于实例的原型， 所有在类中定义的方法， 都会被实例继承。 如果在一个方法前， 加上 static 关键字， 就表示该方法不会被实例继承， 而是直接通过类来调用， 这就称为“静态方法”。  



### class继承

Class 可以通过 extends 关键字实现继承， 这比 ES5 的通过修改原型链实现继承， 要清晰和方便很多。  子类必须在 constructor 方法中调用 super 方法， 否则新建实例时会报错。 这是因为子类没有自己的 this 对象， 而是继承父类的 this 对象， 然后对其进行加工。 如果不调用 super 方法， 子类就得不到 this 对象。  

```javascript
class Point {
    constructor(x,y) {
        this.x=x
        this.y=y
    }
}
class  ColorPoint extends Point{
    constructor(x,y) {
       // super(x,y) 报错了	
    }
}
```

`super` 关键字

`super` 关键字可以当作函数使用，也可以当作对象使用。

当作函数使用，代表着父类的构造函数。ES6要求，子类的构造函数必须执行一次`super`函数。

super 作为对象时， 在普通方法中， 指向父类的原型对象； 在静态
方法中， 指向父类  

## 模块化

在 ES6 之前，社区制定了一些模块加载方案，最主要的有 CommonJS 和 AMD 两 种。前者用于服务器，后者用于浏览器

后来ES6 module出现后，ES6在语言标准的层面上，实现了模块化的功能，完全可以取代CommonJs 和 AMD

### CommonJs

- 使用module.exports  导出 模块 require() 导入模块
- 主要作用于服务端，因为模块是同步加载的，服务端的文件都在本地，读取的时间是硬盘的读取时间，如果在浏览器的使用，模块都放在服务器种，读取的时间取决于网速的快慢，可能等很长时间，浏览器处于假死状态，用户体验不好

### AMD

这就是AMD的产生背景，为了能在模块作用到浏览器中，能让我们采用异步的方式加载模块，模块的加载并不影响后面的语句运行。所有的依赖这个模块的语句，都定义在一个回调函数中，等待加载完成之后，回调函数才会执行。

- 使用define  /require 

### CMD

CMD 是seajs推崇的规范，CMD是依赖就近，也就是说用的时候才requeire 

- AMD和CMD 最大的区别是对依赖模块的执行处理时机不同，二者都是异步加载模块
- AMD推崇依赖前置，在定义模块的时候就要声明依赖的模块，CMD推崇依赖就近，只有用到的某个模块的时候再去require

### ES6 Module

ES6标准出来后， ES6 Moules 规范算是成为前端的主流吧，以 import 引入模块，export 导出模块

### CommonJS与ES6 Modules规范的区别

- CommonJs 模块是运行时加载，ES6 Module编译时输出接口
- CommonJs 输出的是值得拷贝，ES6 Module输出值得引用，被输出模块得内部得改变会影响引用得改变
- CommonJs 导出模块路径可以是一个表达式，因为它是用的是require() 方法，而ES6 Modules只能是字符串
- CommonJs this 指向当前模块，ES6 Module this 指向undefined