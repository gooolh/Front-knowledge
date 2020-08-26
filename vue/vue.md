## MVVM

Model-View-ViewModel 的缩写，Model代表数据模型，View代表试图UI，ViewModel是view和model之间的桥梁，通过双向数据绑定把 View 层和 Model 层连接了起来,数据会自动将数据渲染到页面中，视图变化的时候会通知viewModel层更新数据。

## Vue生命周期

1. beforeCreate   //vue实例创建之前触发一个钩子，在这个阶段data、methods、computed以及watch上的数据和方法都不能被访问。
2. created     //在实例创建完成后发生，这个阶段可以使用data,做一些初始化数据的获取。
3. beforeMount  //发生在挂载之前，当前虚拟dom已经创建完成，即将开始渲染，数据更改不会触发updated
4. mounted    //在挂载完成后发生，数据完成双向绑定，可以使用$refs 进行dom操作
5. beforeUpdate   //发生在更新之前，也就是响应式数据发生更新，虚拟dom重新渲染之前被触发，你可以在当前阶段进行更改数据，不会造成重渲染。
6. updated   //发生在更新完成之后，当前阶段组件Dom已完成更新。要注意的是避免在此期间更改数据，因为这可能会导致无限循环的更新。
7. beforeDestroy   //发生在实例销毁之前，在当前阶段实例完全可以被使用，我们可以在这时进行善后收尾工作，比如清除计时器。
8. destroyed  //发生在实例销毁之后，这个时候只剩下了dom空壳。组件已被拆解，数据绑定被卸除，监听被移出，子实例也统统被销毁。

## 双向绑定

vue的双向绑定是通过数据劫持和发布-订阅模式实现的。

vue2.x使用的是Object.defineProperty 来监听数据的变化，vue3.x使用的是Proxy，因为Proxy可以直接监听对象和数组的变化，并且有多达13种拦截方法。并且作为新标准将受到浏览器厂商重点持续的性能优化。

手写双向绑定 Object.defineProerty方式

```javascript
//超级简单版
//html
<h1 id="name"></h1>
<input type="text">
//js
let obj={}
let el=document.getElementById("name")
let input = document.querySelector('input');
function defineReactive(obj, key, val) {
    Object.defineProperty(obj, key, {
        enumerable: true,
        configurable: true,
        set(v) {
            val = v
            el.innerText=val
        },
        get() {
            return val
        }
    })
}
defineReactive(obj,"name","")
input.oninput = function (e) {
    obj.name = e.target.value
}
```

## computed 和 watch的区别

computed 计算属性，可以当作data一样使用，计算属性的结果会被缓存，除非依赖的响应式 property 变化才会重新计算。注意，如果某个依赖 (比如非响应式 property) 在该实例范畴之外，则计算属性是**不会**被更新的。

watch	一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。Vue 实例将会在实例化时调用 `$watch()`，遍历 watch 对象的每一个 property。

```
computed是依赖vm中data的属性变化而变化的，也就是说，当data中的属性发生改变的时候，当前函数才会执行，data中的属性没有改变的时候，当前函数不会执行。
computed 的值可以当作data一样使用
在computed中不要对data中的属性进行赋值操作。如果对data中的属性进行赋值操作了，就是data中的属性发生改变，从而触发computed中的函数，形成死循环了。
当computed中的函数所依赖的属性没有发生改变，那么调用当前函数的时候会从缓存中读取。

功能上：
computed是计算属性，watch是监听一个值的变化，然后执行对应的回调。
是否调用缓存：computed中的函数所依赖的属性没有发生变化，那么调用当前的函数的时候会从缓存中读取，而watch在每次监听的值发生变化的时候都会执行回调。
是否调用return：computed中的函数必须要用return返回，watch中的函数不是必须要用return。
使用场景：computed----当一个属性受多个属性影响的时候，使用computed-------购物车商品结算。watch----当一条数据影响多条数据的时候，使用watch-------搜索框。
```



## KEY属性的作用

key的作用是尽可能的复用 DOM 元素。

## 组件中的data为什么是一个函数？

一个组件被复用多次的话，也就会创建多个实例。本质上，`这些实例用的都是同一个构造函数`。如果data是对象的话，对象属于引用类型，会影响到所有的实例。所以为了保证组件不同的实例之间data不冲突，data必须是一个函数。

## 虚拟DOM

虚拟`DOM`本质就是用一个原生的`JavaScript`对象去描述一个`DOM`节点。是对真实`DOM`的一层抽象。

由于在浏览器中操作`DOM`是很昂贵的。频繁的操作`DOM`，会产生一定的性能问题，因此我们需要这一层抽象，在`patch`过程中尽可能地一次性将差异更新到`DOM`中，这样保证了`DOM`不会出现性能很差的情况。

另外还有很重要的一点，也是它的设计初衷，为了更好的跨平tai，比如`Node.js`就没有`DOM`,如果想实现`SSR`(服务端渲染),那么一个方式就是借助`Virtual DOM`,因为`Virtual DOM`本身是`JavaScript`对象。

虚拟DOM生成过程：

模板 通过编译->渲染函数 -> 虚拟dom

作用：

- 虚拟DOM的最终目的是将虚拟节点渲染到视图上，但是如果直接使用虚拟节点替换旧节点的话，会有很多不必要的DOM操作，从而导致性能上的浪费。
- 将虚拟节点通过与旧的虚拟节点做比较，通过diff算法做出更新的节点进行dom操作。



## diff算法

- 先同级比较再比较子节点
- 先判断一方有子节点和一方没有子节点的情况。如果新的一方有子节点，旧的一方没有，相当于新的子节点替代了原来没有的节点；同理，如果新的一方没有子节点，旧的一方有，相当于要把老的节点删除。
- 再来比较都有子节点的情况，这里是`diff`的核心。首先会通过判断两个节点的`key、tag、isComment、data同时定义或不定义以及当标签类型为input的时候type相不相同`来确定两个节点是不是相同的节点，如果不是的话就将新节点替换旧节点。
- 如果是相同节点的话才会进入到`patchVNode`阶段。在这个阶段核心是采用双端比较的算法，同时从新旧节点的两端进行比较，在这个过程中，会用到模版编译时的静态标记配合`key`来跳过对比静态节点，如果不是的话再进行其它的比较。

## 通信有哪些方式

父子组件通信

- props方式先子组件传值  
- $emit.on方式先父组件传值
- eventbus方式

兄弟组件通信

- eventbus方式
- vuex

跨组件通信

- vuex

## nextTick

在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

主要为了解决：例如一个data中的数据它的数据会导致视图的更新，而在某个很短的时间被改变了很多次，每一次更新将触发数据中的setter并按流程修改真实DOM，这种做法是很低效的，

## vue性能优化

- v-if和v-show区分场景使用
- v-if和v-for不能连用
- v-for遍历必须添加key,最好为id值
- 图片懒加载和路由懒加载
- 第三方插件按需引用
- v-if和v-for不能连用
- 如果需要使用v-for给每项元素绑定事件时使用事件代理
- SPA 页面采用keep-alive缓存组件

