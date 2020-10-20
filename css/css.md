

## display常用属性值  inline  block inline-block	

- inline 使元素变成行内元素，拥有行内元素的特性，即可以与其他行内元素共享一行，不会独占一行.  不能改变元素的width height  大小由内容撑开，可以使用padding margin 撑开边距 但是top 和bottom 不会起作用
- block  是元素变成块级元素 ，独占一行，块级元素会默认填满父级元素的宽度.  可以改变width和height 
- inline-block  结合了行内元素和块级元素的特性，可以设置padding margin 不受影响，也不会独占一行

## 盒子模型

所有的html元素都可以看作是盒子，一个元素占用空间有元素内容，元素的内边距，元素的边框，元素的外边距，这4个部分一起构成了盒子模型

盒子模型分为 W3C标准盒子  IE盒子

可通过box-sizing 切换盒子模型  box-sizing默认值为content-box 。（为标准盒子） IE盒子属性为 border-box 。

### W3C标准盒子

 又称内容盒子  主流的浏览器都是使用标准盒子模型，它的width一般只包括内容，不包括padding margin 和border,也就是说 盒子的宽度 =margin+border+padding+width

### IE盒子

在该模式下width属性不是内容的宽度，而是padding和border 的总和。

盒子的宽度=margin+width

## position属性

有4个属性

- static	默认值 不脱离文档流  left right bottom top 属性不生效
- relative    相对定位  不脱离文档留   可以根据偏移前的位置作为参照物，进行偏移
- absolute   绝对定位 脱离文档流   绝对定位关键是找参照物   满足条件有 被父元素包裹 且父元素有定位属性
- fixed    固定定位   与绝对定位相似  只不过参照物为浏览器的左上角
- inherit  规定从父元素继承 position 属性的值

## 元素水平左右居中

元素的水平居中很容易实现  如果元素是块级元素，只要将margin属性设为 margin 0 auto; 如果是行内元素，则将text-align设置为center.

实现上下居中就比较麻烦了。有3种方法

- 使用flexbox 布局 使用`justify-content`和`align-items`都设置为`center`
- 位置计算  利用`posttion`属性  将元素`left` 和`top` 偏移50%  再减去本身的宽度和高度的50% `translateX(50%) translateY(50%)`
- 利用` display table`属性

## 什么是BFC（Block formatting  context）

BFC  块级格式化上下文，简单来说它是一个独立的渲染区域，并且有一套渲染规则，决定了子元素如何布局，以及与元素之间的关系和作用，**具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。**

### BFC的布局规则

- 内部的Box会在垂直方向，一个接一个地放置
- 属于同一个BFC的两个相邻Box的margin会发送重叠
- 计算BFC的高度时，浮动元素也参与计算
- BFC的区域不会与float box 重叠
- BFC是一个隔离的独立容器，容器里面的子元素不会影响到外面元素

### 触发BFC的条件

1. float的值不为none
2. overflow的值不为visible
3. position的值为absolute或者fixed
4. display的值为inline-block、inline-flex、flex、flow-root、table-caption、table-cell

### BFC的作用

1. 利用BFC避免margin重叠 （属于同一个BFC的两个相邻的box的margin会重叠，以大的为主。要想解决这个问题，可以将两个盒子分为不同的BFC中）
2. 自适应两栏布局  可以阻止元素被浮动的元素覆盖
3. 清除浮动

## 伪类和伪元素的区别

- 伪类更多的是弥补css选择器的功能，例如选择第一个子元素`:first-child`，一般是1个冒号
- 伪元素是可以在添加一些元素，一般是使用两个冒号

## 单行溢出显示省略号

```css
overflow: hidden;
text-overflow: ellipsis;
white-space: no-wrap;
```

## 多行溢出显示省略号

```css
overflow:hideden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 3;
-webkit-box-orient: vertical;
```

## display: none; 与 visibility: hidden; 的区别

结构

- display:none	会让元素从渲染树中消失，渲染的时候不占据空间
- visibility:hidden   不会让元素从渲染树中消失，渲染元素会占据空间，但不可见，不可点击
- opactiy:0        不会让元素从渲染树中消失，渲染元素会占据空间，不可以，可以点击

继承

- display:none；和opacity:0
  非继承属性
- visibility:hidden
  继承属性，子孙节点消失由于继承了hidden,可以修改子孙节点属性，可以子孙显示

## 圣杯布局和双飞翼布局

实现两侧宽度固定，中间内容宽度自适应的三栏布局

### 圣杯布局：

​	优点：不需要添加额外dom节点

​	缺点：正常请求下是没问题的，如果当内容的宽度小于左栏的宽度的时候，布局就会发生混乱，圣杯破碎

### 双飞翼布局：

​	需要在内容层添加一层包裹

### 实现：

1. 使用float布局，将margin 设置为负值，将container 设置内边距padding，等于左右两栏的宽度，左右两栏设置left和right偏离原来的宽度
2. 使用flex布局，将外层容器设置flex容器，和将容器设置flex为1就ok了
3. 使用grid布局，



## 预处理器

CSS 预处理器定义了新的语言，为CSS增加了一些编程的特性，然后再编译成CSS文件，css预处理器提高了很多遍历，例如可以在CSS中使用便利，比如是CSS中使用变量，简单的逻辑程序、函数等一些编程语言的一些基本特性，可以让你的CSS更加简洁、兼容性更强等

## GPU加速

GPU加速原理是单独给元素生成一个复合图层，不管这个图层怎么变化，也不会影响默认的复合图层

可使用的元素有

- transform
- opacity
- will-change  提前告诉浏览器要准备好优化，因为将一个元素提升为了一个图层，是比较慢的，使用这个属性可以让浏览器提前做好准备