## npm

- nodejs的包管理仓库

```javascript
-i  //install
-D  //--dev  安装到开发依赖，
-S 	//--save	安装到依赖
-g	//global	全局安装
```



## yarn

- 相对比与NPM，安装速度快 (服务器速度快 , 并且是并行下载)

与NPM语法对比

| npm语法                     | yarn语法            | 解释               |
| --------------------------- | ------------------- | ------------------ |
| npm install  xxx --save     | yarn add xxx        | 向生成环境添加依赖 |
| npm install  xxx --save-dev | yarn xxx --dev      | 向开发环境添加依赖 |
| npm intall -g xxx           | yarn global add xxx | 全局安装           |
| npm updata xxx              | yarn upgrade        | 更新包             |
| npm uninstall xxx           | yarn remove xxx     | 移除包             |
| npm install                 | yarn (install)      | 安装依赖包         |



## npx 

- 调用项目安装的模块,一般来说我们调用项目的脚本，如果在命令行上使用则需要像这样

  ```
   node-modules/.bin/webpack xxxxx
  ```

  npx就是想解决这个问题，让项目内部安装的模块使用更加方便，只需要这样

  ```
  npx webpack
  ```

  

- 避免全局安装 (不用更新原来的包的版本，直接拉取最新版本) ,比如react的手脚架可能会更新版本，如果我们全局安装的话，像这样

  ```javascript
  npm -g create-react-app  //全局安装而没有及时更新的话会导致我们的版本过久，
  create-react-app my-app  
  ```

  官网推荐使用npx

  ```
  npx create-react-app react-app
  ```

  

- 灵活选择node的版本下的包