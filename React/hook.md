## React  Hook 

​	Hook 是React 16.8的新特性，可以不编写class的情况下使用state以及其他React 的生命周期

### 为什么需要Hook？

- 组件之间复用状态逻辑很难
- 复杂组件变得难以理解
  我们经常会维护一些组件，随着组件越来越复杂，状态逻辑和副作用会，生命周期会包含一些不相干的逻辑，又把逻辑相关的分离到不同的方法中，会导致逻辑上的混乱，**Hook将组件互相关联的部分拆分成更小的函数，（比如设置订阅或请求数据），而并非强制按照生命周期划分**
- class组件存在的一些问题
  比如我们需要理解javaScript中的`this`的工作方式，还不能忘记给绑定事件绑定`this`，这些代码会非常的冗余

**为了解决这些问题，Hook使你在使用非class的情况下可以使用更多的React特性**

### useState

​	返回一个 state，以及更新 state 的函数

### useEffect

​	首次渲染和更新都会执行的函数，可以用来模拟`compoentDidMount` `compoentDidUpdate` `compoentWillUnmount`，返回一个函数（effect可选的清除机制）

### useMemo

​	**返回一个缓冲的值**

​	在hook产生之前，我们知道 

- 调用setState ，就会触发组件的重新渲染，无论前后的state是否相同
- 父组件更新，子组件也会发生更新

对于以上两点，我们通常会使用`immutable`进行比较，在不相等的时候调用`setState`，或者在`shouldComponentUpdate`中判断前后的prps和state,如果没有变化，则返回false来阻止更新。

在Hook出现之后，我们使用了function的方式创建组件的，这是就会用到`useCallback`和`useMemo`来优化性能

我们希望只有但我们依赖的数据发生更新时，我们会发生更新，其他的情况下则返回上一次缓冲中的值

### useCallback

​	和useMemo的区别是 **返回一个缓冲的函数**，可以把回调函数传给子组件，来避免给必要的更新（例如shouldCompoentUpdate）

### useContext

​	