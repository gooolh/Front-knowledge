## 设备独立像素

- 在之前手机像素不是很高的时候，1px是等于1个像素的，随着手机屏幕的发展，手机尺寸大小不怎么变，分辨率越来越高，像素密度越来越高.引入了独立像素的概。
- window有个属性devicePixelRatio(dpr)，描述了物理像素和设备独立像素的比例，设备独立像素就是css的px，1px不是等于屏幕上的1个像素点，它是一个抽象的概念，在不同的设备在1px代表的物理像素并不相等的，devicePixelRatio是有系统所决定的，比如在iphone中devicePixelRatio 2或者3，取决于手机型号。

## 移动端1px问题

- 移动端1px并不是真正的1个像素，它是等于1px*dpr个物理像素，如果想要`1px`的话就要进行缩放处理。
- 解决的方案大致有：用小数、用图片、用渐变、用阴影、用 transform 缩放。手机淘宝的做法是使用 js 动态设置 viewport 的 initial-scale。参考（https://www.jianshu.com/p/7e63f5a32636）

## CSS单位rem

在[W3C](http://www.w3.org/TR/css3-values/#rem-unit)规范中是这样描述`rem`的:

> font size of the root element.

简单的理解，`rem`就是相对于根元素`<html>`的`font-size`来做计算。而我们的方案中使用`rem`单位，是能轻易的根据`<html>`的`font-size`计算出元素的盒模型大小。而这个特色对我们来说是特别的有益处。

rem的计算

假如设计稿为750px，分成15分(划分标准不唯一可以是20份也可以是10份)

每一份作为html的字体大小，这里就是50px

那么在320px设备的时候，字体大小就是320/15 = 21.33px

用我们页面元素的大小除以不同的html字体大小会发现它们比例还是相同的

 页面元素的rem值 = 页面元素值px / html字体大小

参考文章 （https://github.com/amfe/article/issues/17）

