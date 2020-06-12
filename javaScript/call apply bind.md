# 实现call() , apply() , bind()

### 实现call(obj,arg,arg)  将目标函数的this指向传入第一个传入的对象

实现思路

1. 改变this的指向
2. 利用arguments类数组对象实现参数不定长
3. 不能增加对象的属性，所以在结尾需要delete

```
Function.prototype.$call=function (target) {
    var args = Array.prototype.slice.call(arguments,1);  //利用Array slice获取从1后面的函数
    target.fn=this  //改变函数this的指向   因为JavaScript中this的指向不是创建时确定的，时调用时确定的
    target.fn(...args) 
    delete target.fn
}
```



## 实现apply

1. 与call（）只有一个区别，apply第二个参数为数组

```
Function.prototype.$apply=function (target) {
    target.fn=this
    target.fn(...arguments[1])
    delete target.fn
}
```

## 实现bind

1. bind 返回一个函数，第一个参数为this指向的对象，后面为参数
2. 改变this的指向 可以用call apply
3. 使用concat 把参数数组拼接，

```
Function.prototype.$bind=function (target) {
    const _this=this
    const args=[].slice.call(arguments,1)
    const bound=function () {
        const boundArgs =[].slice.call(arguments)
        return _this.apply(target,args.concat(boundArgs))
    }
    return bound
}
```

