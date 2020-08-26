## 常用Loader

- flie-loader：把文件文件输出到一个文件夹中，在代码中通过相对的URL去引用输出文件
- url-loader：与file-loader相似，区别是用户可以设置一个阈值，大于的阈值的话交给file-loader处理，小于的话时则返回base64编码，利用这个url-loader，可以把小一点图片，转成base64 形式，减少http请求
- source-map-loader：加载额外的 Source Map 文件，以方便断点调试
- babel-loader：把ES6转成ES5
- ts-loader：把TypeScript 转成 javaScrpt
- awesome-typescript-loader：将 TypeScript 转换成 JavaScript，性能优于 ts-loader
- sass-loader：把scss/sass 转换成css
- css-loader：加载css,支持模块化、压缩、文件导入等特性
- style-loader：把css代码注入到JavaScript,通过dom操作加载css
- postcss-loader：扩展css语法，可以配置autoprefixer插件自带补齐浏览器前缀
- eslint-loader：通过 ESLint 检查 JavaScript 代码
- tslint-loader：通过 TSLint检查 TypeScript 代码
- vue-loader：加载Vue.js 单文件组件

## 常用plugin

- Ignore-plugin：忽略打包的部分文件
- html-webpacl-plugin：简化HTML文件创建
- clean-webpack-plugin：目录清理
- uglifyjs-webpack-plugin：代码压缩，不支持ES6压缩
- terser-webpack-plugin：支持ES6压缩
- webpack-paraller-uglify-plugin：多进程执行代码压缩，提高构建速度
- mini-css-extract-plugin：分离样式，将CSS提取到独立文件
- speed-measure-webpack-plugin：可以看到每个Loader和Plugin执行时间，打包耗时
- webpack-bundle-analyzer：可视化webpack的输出文件的体积

## Loader和Plugin的区别

Loader本质是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果，因为webpack只认识JavasScript,所以Loader就成了翻译官，对其他资源进行转译的预处理工作

Plugin就是插件，插件可以扩展Webpack的功能，在webpack运行的生命周期会广播许多事件，Plugin可以监听这些事件，进行处理。

## Wenpack的热更新原理

webpack的热更新又称热替换，这个机制可以做到不用刷新浏览器而将变更的模块替换掉旧的模块

HMR的核心就是客户端从服务端拉取更新后的文件，实际上webpack-devServce与浏览器之间维护了一个websocket,当我们源文件发生变化时，WDS会向浏览器推送更新，并带上构建的hash，让客户端与上一次资源进行对比，客户端对比出差异后会向WDS发起一个ajax请求，获取更改hash，这样客户端可以通过这些信息继续向WDS 发起`jsonp`请求获取chunk的更新

后续的部分(拿到增量更新之后如何处理？哪些状态该保留？哪些又需要更新？)由 `HotModulePlugin` 来完成，提供了相关 API 以供开发者针对自身场景进行处理，像`react-hot-loader` 和 `vue-loader` 都是借助这些 API 实现 HMR。



## 文件hash

文件Hash是打包后输出的文件名的后缀，可以帮助我们及时获取最新的修改文件，和命中缓存，文件指纹有3种

1. Hash：与整个项目的构建相关，只要项目文件有修改，整个项目构建的hash值会更改
2. Chenkhash：和webpack打包的chunk有关，不同的entry会产生不用的chunkhash
3. contentHash：根据文件的内容来定义hash，文件内容不变，则contentHash不表



## 构建优化

