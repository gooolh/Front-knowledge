

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

## 元素水平左右居中

元素的水平居中很容易实现  如果元素是块级元素，只要将margin属性设为 margin 0 auto; 如果是行内元素，则将text-align设置为center.

实现上下居中就比较麻烦了。有3种方法

- 使用flexbox 布局
- 位置计算  利用posttion属性  将元素left 和top 偏移50%  再减去本身的宽度和高度的50% translateX(50%) translateY(50%)
- 利用 display table属性



