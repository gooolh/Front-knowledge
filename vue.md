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

```

```



## 虚拟dom

## diff算法

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

- 尽量减少data中的数据，data中的数据都会增加getter和setter，会收集对应的watcher
- v-if和v-for不能连用
- 如果需要使用v-for给每项元素绑定事件时使用事件代理
- SPA 页面采用keep-alive缓存组件
- 在更多的情况下，使用v-if替代v-show
- key保证唯一
- 使用路由懒加载、异步组件
- 防抖、节流
- 第三方模块按需导入
- 长列表滚动到可视区域动态加载
- 图片懒加载
- 将一些常用的方法封装成library发布到npm上，然后在多个项目中引用
- 也会将一些公用的组件封装成一个单独的项目然后使用webpack打包发布到npm上