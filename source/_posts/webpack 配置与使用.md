---
title: webpack 配置与使用
date: 2021-12-13 16:04:15
tags:
  - webpack
  - 工具
categories:
  - 工具使用
---

![webpack](https://gitee.com/Rexiamu/image-hosting/raw/master/img/20211213144846.png)

本质上，**webpack** 是一个用于现代 JavaScript 应用程序的 *静态模块打包工具*。当 webpack 处理应用程序时，它会在内部从一个或多个入口点构建一个 [依赖图(dependency graph)](https://webpack.docschina.org/concepts/dependency-graph/)，然后将你项目中所需的每一个模块组合成一个或多个 *bundles*，它们均为静态资源，用于展示你的内容。

<!-- more -->

## 作用

- 模块化管理。在写项目时我们可以分成许多模块以降低模块的复杂性，同时也利于我们的维护。通过  **webpack** 打包我们又可以将这些模块整合起来。
- 兼容处理。部分 **javascript** 比较新的语法可能在个别浏览器上无法得到支持，此外还有便于开发用的 **less** 等升级语言，所以我们要靠 **webpack** 将代码编译为浏览器所支持的语言。
- 插件。 **webpack** 中的 **plugins** 支持我们使用一些插件对项目进行优化，如按需加载，代码压缩等。

## 介绍

在项目根目录新建 **webpack.config.js** 文件用来存放 **webpack** 配置信息。

```js
module.exports = {
	// 配置信息
}
```

### mode

编译模式。分为 **`development`** 和 **`production`**：

- `development  开发模式：速度快、体积大`
- `production  生产模式：数度慢、体积小`

```js
module.exports = { 
  mode: 'development',
}
```

### entry

指定要处理的文件

```js
// 文件路径工具
const path = require('path')
module.exports = { 
  // path.join()  拼接路径
  // __dirname  当前目录
  entry: path.join(__dirname, './src/index.js')
}
```

### output

文件输出位置

```js
// 文件路径工具
const path = require('path')
module.exports = { 
  output: {
    // 存放目录
    path: path.join(__dirname, 'dist'),
    // 生成的文件存放路径
    filename: 'js/bundle.js'
  }
}
```

### plugins

插件使用。将配置好的插件实例放在 **plugins** 数组中。

```js
// html 插件，该插件会自动在 index.html 文件中引入生成的文件
const HtmlPlugin = require('html-webpack-plugin')
// 删除打包文件的插件
const { CleanWebpackPlugin } = require('clean-webpack-plugin')  
// 创建 html 插件实例  
const htmlPlugin = new HtmlPlugin({ 
  template: './src/index.html', // 指定原文件的存放路径 
  filename: './index.html', // 指定生成文件的存放路径  
}) 
module.exports = { 
  plugins: [htmlPlugin, cleanWebpackPlugin],
}
```

#### html-webpack-plugin

打包时创建入口文件，一般为 **index.html**。此文件为项目的入口文件，通过访问此文件来浏览项目。

#### clean-webpack-plugin

项目每次打包都会生成一套文件，而每次的文件名会有差异，这就需要我们每次生成前删除之前的文件。这个插件可以帮助我们做这部分的工作。

### devServer

开发服务配置。

```js
module.exports = { 
  // 开发服务配置
  devServer: {
    // 自动打开浏览器
    open: true,
    // 端口号
    port: 80,
    // 主机地址
    host: '127.0.0.1', // 默认：localhost
  }
}
```

### loader

加载器。**webpack** 默认只支持 **javascript** 编译，若想要编译 **css**、图片等的编译需要加入对应的加载器。

```js
module.exports = { 
  // 第三方模块配置
  module: {
    // 文件处理规则
    // 倒序执行 use 数组里的加载器
    rules: [
      // css
      { test: /\.css$/, use: ['style-loader', 'css-loader'] },
      // less
      { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
      // 图片
      // limit  图片大小限制  单位：byte(字节)  <= limit 时会将图片转换为 base64 格式，否者为原来格式
      // outputPath  指定图片文件输出路径
      { test: /\.jpg|png|gif$/, use: 'url-loader?limit=50000&outputPath=images' },
      // 使用 babel 处理 js 高级语法
      // 第三方包即（node_modules）不需要处理
      { test: /\.js$/, use: 'babel-loader', exclude: '/node_modules/' },
    ]
  }
}
```

### devtool

调试工具选项，可以帮助我们定为代码报错位置。

> SourceMap 源代码和生成代码的对应关系

```js
module.exports = { 
  // 开启后可以定位到发生错误的源代码具体行数
  // nosources-source-map  显示行号但不显示源代码
  // eval-source-map  显示行号并且显示源代码
  devtool: 'eval-source-map'
}
```

### alias

别名。解决层级复杂的问题，例如：**"../../../../images/logo.jpg"** 。使用别名指向 **src** 目录后即可用 **"@/images/logo.jpg"** 访问。

```js
module.exports = { 
  resolve: {
    // 别名，项目中使用 @ 将被替换为根目录的 src
    alias: {
      '@': path.join(__dirname, './src/')
    }
  }
}
```
