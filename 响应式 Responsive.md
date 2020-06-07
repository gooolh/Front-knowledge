### 响应式布局

- 使用媒体查询，对于不同屏幕尺寸进行修改。

  ```
  @media screen and (min-width:768px) 小屏幕使用
  @media screen and (min-width:768) and (max-width:992) 平板
  @media screen and (min-width:992) and (max-width:1200px) 桌面
  @media screen and (min-width:1200px) 桌面
  参考了bootstrap的样式
  ```

  

- 设置meta viewport ，在默认的浏览器，viewport默认值一般为960px，然而手机的屏幕并没有这么打，导致网页被缩了。将viewport设置为width=device-width为屏幕的宽度，由于网页的宽度大于屏幕宽度，没有是适配移动端的网页会出现滑动条的。

- 图片的解析度,由于现在的屏幕都是高清屏(retina iphone提出的高大上的名称) dpr一般大于1，一般为2或3，了我们可以想象在2种不同的手机里有同样的画面，一个是普通显示屏，另一个是高清屏幕，在同样大小的屏幕上，高清显示屏中的位图会被放大，图片会变得模糊。所以需要在不同dpr进行处理

  ```
  <picture>
    <source srcset="https://placehold.it/600x600 2x,https://placehold.it/900x900 3x">
    <img src="https://placehold.it/300x300">
  </picture>
  ```

  

- 使用rem布局 