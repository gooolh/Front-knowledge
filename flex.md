## Flexbox  网页布局

​	同学们，早上好，我是你们的老师刘锦桃，今天我要讲的是制作网页的很重要的知识点，Flexbox 网页布局，flex布局呢，目前是很流行的，特别是在移动端。很多css框架都是用FlexBox作为基础，浏览器的兼容情况也很不错。好,我们来开始我们学习

好 我们看一下例子 ，广师大的官网 可以看出它 有头部区 有菜单导航区域 还有中间的内容区域，这些是可以通过css来控制它的布局方式的的，比如还有一些网站是有侧边栏的和底部区域各种布局，这些呢 如果掌握了flex 布局，实现起来就比较简单了。



常用的布局方式有传统布局方式 ，以往我们制作网页的时候，会使用display,float等等方式去处理网页的布局，但是其实这些发明出来的时候，都不是专门用于网页排版的用途的，它对于那些特殊布局非常不方便，比如，[垂直居中](https://css-tricks.com/centering-css-complete-guide/)就不容易实现。我想问下同学们，垂直居中你们会使用哪种方法呢?   哎 有同学回答用posittion absolute 再将元素left top 偏移50% 和减去本身的宽度和高度 ，还有同学提到了 可用display table 再将vertical-align 设置为middle

   FlexBox是专门用于网页布局的css方法 。对于垂直居中处理，而使用flexBox非常很容易实现了。



1:移动端有许多尺寸大小，flex能够根据它的尺寸大小 弹性伸缩 页面更加好看自然

2: 也就是子元素可以各种对齐，非常灵活

好 我们仔细来看一下这张图，flex container 有2个重要的点 

1. 每个弹性容器都有两根轴：**主轴和交叉轴**，两轴之间成90度关系。注意：**水平的不一定就是主轴。**
2. 每根轴都有**起点和终点**，这对于元素的对齐非常重要。
3. 弹性元素也可以通过`display:flex`设置为另一个弹性容器，形成嵌套关系。因此**一个元素既可以是弹性容器也可以是弹性元素**





容器的属性有6个  这里我就简单介绍一下   下面会有演示来讲解一下实际的使用







容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做`main start`，结束位置叫做`main end`；交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`。

项目默认沿主轴排列。单个项目占据的主轴空间叫做`main size`，占据的交叉轴空间叫做`cross size`。

1. Flexbox的布局方式主要分为2个角色，一是Flex Container,二是Flex item.

Flex-Container

1. 首先是第一个部分 flex container 的设定
2. flex-direction 是用来定义 在它里面flex items的排序方向，它的默认值为row 就是横向的意思 column就是直向的意思，除了row 和column 还有row-reverse 和 column-reverse 意思是倒转它们的排序顺序
3. 还有一个很重要的概念，就是主轴(main axis) 和交叉轴 (cross axis) 当你的flex-direction是row的时候 既是row是主轴，column是交叉轴。当是flex-direction是column的时候,主轴是column，row是交叉轴
4. justify-content 是设置主轴的排列方向  align-items是设置交叉轴的排列方向，将justify-content 设置为center的时候，是横向方式居中对齐，align-item 设置为center 就是垂直方向 居中对齐 常用的值还有flex-start 和flex-end 
5. flex-warp 默认值是nowrap 就是不换行的意思， 它的值有wrap  换行的意思
6. flex-flow 就是flex-direction 和flex-wrap 的缩写 如果flex-flow 为row nowrap它的意思就是flex-diection row flex-wrap 为nowrap的意思 大家可以视乎习惯，分开或组合一起写也是可以的
7. 

f