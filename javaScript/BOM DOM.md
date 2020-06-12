### BOM  浏览器对象模型（Browser Object Model，简称 BOM）与浏览器窗口交互的对象

1. windows	浏览器的窗口，全局作用域，全局变量都是该变量的属性
2. location	浏览器的当前地址信息，可以获取当前地址信息和设置当前地址
3. navigator 浏览器信息
4. screen 屏幕信息
5. history  浏览器的历史信息，通过history实现上一步或下一步



### DOM 文档对象模型（Document Object Model）

文档对象模型 (DOM) 是HTML和XML文档的编程接口，程序可以对该结构进行访问，从而改变文档的结构，样式和内容。

操作dom意思是改变html的元素

获取元素

```
document.getElementById()	//根据id获取
document.getElementByclassName() //根据class名获取，返回一个元素集合
document.getElementByTagName()	//根据标签名获取，返回一个元素集合
document.querySelector() //根据选择器来获取，返回找到第一个元素
document.querySelectorAll() //返回全部

常用熟悉
element.nodeType	//1为元素节点的标志 2:属性节点	3:文本节点
element.childNodes	//获取元素节点和文本节点
element.children	//只获取元素节点
element.innerText  //获取或设置标签的文本
element.innerHTML	//获取文本或者设置文本，标签
```

修改元素

```
// 括号传入属性名，返回对应属性的属性值 一般是获取自定义属性
element.getAttribute(attributeName)
// 传入属性名及设置的值
element.setAttribute(attributeName,attributeValue)
//设置样式
element.style.样式名称=值
//设置class
element.className=""
element.classList.add(cls) 添加class
element.classList.remove(cls) 移除class
element.classList.contains(cls) 是否存在某个class
element.classList.toggle(cls) 切换某个class
element.classList.toString() 字符串形式输出所有class
获取data属性
element.dataset.toggle 获取data-toggle属性
element.dataset.xxxYyy 获取data-xxx-yyy属性
```

创建元素节点

```
document.createElement(标签名)  //创建元素节点
elememt.append(ele)  //在元素末尾添加元素
element.preend(ele)	//在元素头部添加
```

删除节点

```
element.removeChild(Node)
```



属性事件

```
0级事件 定义一个监听函数，参考(https://www.cnblogs.com/chenyingying0/p/12129182.html)
element.onclick=function(){...}
2级事件 可以绑定多个监听函数，不被覆盖，还有捕获和冒泡控制
element.addEventListener('click', function() {
    alert("DOM2级事件绑定方式")
}, false)

鼠标事件
click //点击事件
dblclick //双击事件
mousedown //鼠标按下
mouseup   //鼠标松开
mouseout  //鼠标移出元素，包括子元素
mouseover //鼠标移入元素，保护子元素
mouseleavve //鼠标移出元素
mouseenter //鼠标移入元素
```

