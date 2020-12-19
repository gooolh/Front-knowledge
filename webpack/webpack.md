## 前端为何要进行打包和构建

- 统一高效的开发环境
- 统一的构建流程和产出的标准
- 集成统一的代码规范（测试、上线等）

代码产出

- 体积更小（Tree-Shaking、压缩、合并），加载更快
- 编译高级语言或语法（TS、ES6+、模块化、scss）
- 兼容性和错误检查（polyfill、postcss、eslint）

## webpack是什么

webpack是一个模块打包器，通过webpack将零散的模块打包成一个javascript文件，对于代码有兼容性问题，通过loader将其转化，webpack还可以进行代码分离解决bundle文件过大问题，对公共代码代码一个chunk或者使用import语法将其按需加载，因为并不是每个模块启动都是必要的，对于引用而为使用的代码，webpack会有一个tree-shaking，打包时不会被打包上去

## webpack流程

1. 初始化参数：从配置文件和shell命令读取并且合并参数
2. 开始编译：用上一步得到的参数初始化complier对象,加载插件,并执行对象的run方法开始编译
3. 确认入口：找到配置文件的entry
4. 编译模块：从入口文件出发，调用配置的Loader对模块开始编译，再找到模块依赖的模块，递归本步骤直到所有的入口文件都被编译完成
5. 完成模块编译：得到每个模块的编译后的内容和它们的依赖关系
6. 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的Chunk，再把每个Chunk转换成文件加上输入列表，这是修改输出内容的最后机会
7. 输出完成：根据配置输出的路径和文件名，写入文件系统

## 常用Loader

- flie-loader：把文件输出到一个文件夹中，在代码中通过相对的URL去引用输出文件
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
- html-webpack-plugin：简化HTML文件创建
- clean-webpack-plugin：目录清理
- uglifyjs-webpack-plugin：js代码压缩，不支持ES6压缩
- terser-webpack-plugin：支持ES6压缩
- webpack-parallel-uglify-plugin：多进程执行代码压缩，提高构建速度
- mini-css-extract-plugin：分离样式，将CSS提取到独立文件
- speed-measure-webpack-plugin：可以看到每个Loader和Plugin执行时间，打包耗时
- webpack-bundle-analyzer：可视化webpack的输出文件的体积
- HappyPack 开启多进程打包

## Loader和Plugin的区别

Loader本质是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果，因为webpack只认识JavasScript,所以Loader就成了翻译官，对其他资源进行转译的预处理工作	

Plugin就是插件，插件可以扩展Webpack的功能，在webpack运行的生命周期会广播许多事件，Plugin可以监听这些事件，进行处理。

## Wenpack的热更新原理

webpack的热更新又称热替换，这个机制可以做到不用刷新浏览器而将变更的模块替换掉旧的模块

HMR的核心就是客户端从服务端拉取更新后的文件，原理是webpack-devServce与浏览器之间维护了一个websocket,当我们源文件发生变化时，WDS会向浏览器推送更新，并带上构建的hash，让客户端与上一次资源进行对比，客户端对比出差异后会向WDS发起一个ajax请求，获取更改后的文件hash，这样客户端可以通过这些信息继续向WDS 发起`jsonp`请求获取chunk的更新

后续的部分(拿到增量更新之后如何处理？哪些状态该保留？哪些又需要更新？)由 `HotModulePlugin` 来完成，提供了相关 API 以供开发者针对自身场景进行处理，像`react-hot-loader` 和 `vue-loader` 都是借助这些 API 实现 HMR。

## babel

​	将ES6或者更高级别的语法转成兼容性更好的ES6语法

配置`perset` ,预设是通用插件的一个组合，将常用的`ES6 ES7 等等`新特性转换的插件集合起来，不用我们一个个配置

`polfill` 补丁，比如`promise` IE浏览器不支持，可以通过打补丁的方式，插入当中，使得它支持，核心`core.js regenerate`，`core.js`是常用的一个补丁的一个集合，`regenerate`是支持 ES6中的`generate`语法

`babel-polyfill` 是两者的集成，如果直接引用的话，会造成全局污染，打包的代码体积增大，我们可能只是用部分的功能，无需全部引用，

按需引用 `babel-polyfill`  

使用`babel-runtime` 和`babel-plugin-transform-runtime`相结合，

`babel-runtime` 我需要什么api 则引用什么api，它并不是直接取代我们之前的已有的`api`，而是使用换名字后的`api`，这样解决了全局污染的，`babel-plugin-transform-runtime`可以帮助我们自动`import`，不需要一个个去申明

## 文件hash

文件Hash是打包后输出的文件名的后缀，可以帮助我们及时获取最新的修改文件，和命中缓存，文件指纹有3种

1. Hash：与整个项目的构建相关，只要项目文件有修改，整个项目构建的hash值会更改
2. Chenkhash：和webpack打包的chunk有关，不同的entry会产生不用的chunkhash
3. contentHash：根据文件的内容来定义hash，文件内容不变，则contentHash不表

## Scope hosting

 	作用域提升,依赖于ESM 语法，分析出模块的之间的依赖关系，尽可能将分散的函数合并成一个函数中，但是前提不能造成代码的冗余，因此只有那些被引用一次的模块才可能被合并。

使用：这是webpack的内置的功能，只需要配置一个插件 `MouleConcatenationPlugin` ，由于许多npm包采用`CommonJs`语法，也有一部分库采用了`ESM`为了充分发挥出`scope hosting`作用，好需要配置一个` mainFields: ['jsnext:main', 'browser', 'main']`

好处：代码体积减少，代码运行时创建的作用域减少，内存开销减少

## 性能优化

- 使用production模式 会自动开启 tree shaking
- scope hosting 减少代码体积
- IngorePlugin 排除不需要打包的文件，文件体积更小
- 小图片base64编码 ，可以减少网络请求
- 代码分离，将第三方的包，单独打包出来，利用contentHash加上文件指纹，再配合浏览器强缓存，一般只有修改的代码才需要再一次请求
- 懒加载，利用import() 语法，webpack打包的时候将代码单独分隔出来一个js文件，等需要用到时再通过请求加载进来
- 使用CDN加速，利用wenpack的externals配置，将第三方的包用CDN

## 构建优化

- 使用**exclude **缩小构建目标
- 速度分析
  webpack有时候打包很慢，我们的项目中可能用到很多loader和pluhin,我们想知道哪个环节打包慢，可以使用**speed-measure-webpack-plugin**插件，计算耗时
- 体积分析
  可以采用 **webpack-bundle-analyzer** 插件分析包体积，我们可能引用第三方组件库过大，这时候是否需要寻找替代品 
- **babel-loader** 使用cacheDirectory 缓冲，将转义后的结果缓冲到文件中，之后再构建就会尝试读取缓冲了
- **cache-loader** 不仅babel-loader 可以缓冲，其他loader也可以缓冲起来
- 使用**DLL 动态链接库** 可以将经常复用的模块编译成dll，一般是一些第三方的模块 比如 vue react react-dom 只要不升级这些库，动态链接就不需要重新编译
- **HardSourceWebpackPlugin** ,为模块提高中间缓冲
- 多进程构建
  **thread-loader**
- 多进程并行压缩代码
  **terser-webpack-plugin** 插件 可选参数parallel 进程数量 
- 利用缓存提升二次构建速度
  

## 浏览器兼容

- 浏览器前缀：利用postcss插件 为不同浏览器的添加css的前缀。
- 老的浏览器：利用babel-loader，可以将ES6/ES7/ES8等，一些新的语法转换成ES5 大家都能识别的语言，再有babel-polyfill垫片，对于一些不支持的新特性的对象，进行实现，babel-polyfill有全局的污染，如果我们使用新特性，也会打包在里面，我们可以使用babel-runtime和babel-tranform-plugin
- 使用postion和float布局兼容性好，flex布局和grid布局对于IE10一下都不支持了 