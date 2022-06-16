---
title: think of webpack
date: 2022-05-23 00:46:01
tags: [webpack,javascript module]
categories: webpack
---

# webpack 配置

## output

> 问题1

   近期在配置 webpack 时,发现 output -> libraryType: 'umd' or output -> library: {type: 'umd'} 打包构建导出的模块无法实行 TreeShaking.为什么?

   - 介绍.

     在配置 webpack 的输出 output 时,都会遇到输出模块的配置,也就是 webpack4 中的 libraryType 以及 webpack5 中的 libraryType(未删但新特性不更新) and library: {type: 'xxx'},首先解释一下这几个属性的含义,libraryType 指的是每个 chunk 模块以怎样的模块形式构建打包输出,而 library: {type: 'xxx'} 在webpack5 中属于对 libraryType 的复写,跟 libraryType 是一个含义.
   
     * libraryType 的值枚举.

       ```javascript
        module.exports = {
          //...
          //值枚举
          //'commonjs' | 'commonjs2' | 'amd' | 'umd' | 'this' | 'var' | 'global' | 'module'
          output: {
            libraryType: 'commonjs' 
          }
          //...
        };
       ```

     * libraryType 中包含模块导入导出的值枚举.

       'commonjs' | 'commonjs2' | 'amd' | 'umd' | 'module' 这五个值枚举代表了模块导入导出历史的发展轨迹,由最初的 commonjs 到最终方案 esm,关于它们的含义以及对比,这里把它放在了<a href='https://white-than-wood.github.io/2022/05/23/thinkofjsmodule/#%E6%A8%A1%E5%9D%97%E5%AF%BC%E5%85%A5%E5%AF%BC%E5%87%BA%E7%9A%84%E5%8E%86%E5%8F%B2'>think of JsModule#模块导入导出的历史</a>中,这部分非常重要,是为后续下文的理解做铺垫.
   
     * TreeShaking.

       TreeShaking 最初是 <a href='https://rollupjs.org/guide/en/'>RollUp</a> 团队推出的一个方案,以 esm 模块导入导出模式的静态分析为依赖基础,在编译时对项目代码进行瘦身,将无用、空引入的代码模块不构建进打出的包中,减少代码体积,减少项目冗余,提高项目运行速度的模式.
   
     * import umd.

       对于 umd 的 esm import 导入,在 commonjs、NodeJS 环境下都可以实现,但是必须注意 <a href='https://white-than-wood.github.io/2022/05/23/thinkofjsmodule/#%E6%A8%A1%E5%9D%97%E5%AF%BC%E5%85%A5%E5%AF%BC%E5%87%BA%E7%9A%84%E5%8E%86%E5%8F%B2'>esm(ecmascript module)</a> NodeJS ESM部分.

   - 结论.

     由于上述 umd 所兼容的所有模块导入导出模式不能实现静态分析,包括 import 配合 module.exports (使用 esm import 导入 commonjs2 值枚举导出的模块),而 TreeShaking 又是以 esm 模块导入导出模式的静态分析为依赖基础的,所以 umd 模块导入导出模式不能实行 TreeShaking.
   
## 命令行

> 问题1

  webpack5 中将命令行参数 --display-modules、--display-reasons 删掉了吗?
  
  - 介绍.

    在 webpack5 中执行 webpack --display-modules --display-reasons 会报错.
  
        [webpack-cli] Error: Unknown option '--display-modules'
        [webpack-cli] Run 'webpack --help' to see available commands and options
  
  - 原因.

    webpack5 对日志方面进行了规整,当然输出所有的模块以及输出所有的模块信息的原因也不例外,--display-modules、--display-reasons 迁移到了 <a href='https://webpack.js.org/configuration/stats/'>stats</a> 日志配置中,请查看 stats.modules、status.reasons.

> 问题2

  在 package.json 中的 scripts 属性直接写 shell 脚本,为什么可以在命令行中直接执行而无需任何的目录前缀?

  - 介绍.

    一般在使用 npm 的项目中,直接使用 node_modules/.bin 目录中的可执行文件即可运行 shell 脚本(bash 命令),那既然是这样,在 package.json 这个描述外部依赖的 json 文件中,直接 scripts 属性写 shell 脚本,接着 npm run \<scripts属性\> 就可以直接运行可执行文件而无需通过 .bin 目录呢?

  - 原因.

    实际上在运行 npm run \<scripts属性\> 时,npm 会新建一个 shell 脚本,并把所对应的命令直接添加到 shell 脚本中执行,且将 node_modules/.bin 的子目录临时添加到 $PATH 全局变量中,执行完之后再恢复原状,故无需通过添加目录前缀直接执行.

    PS: node_modules/.bin 目录中的可执行文件都是通过软链的方式连接依赖项目中的可执行文件的.

## module

### enforce

> 介绍

  module.rules 中的 enforce 属性可以设定 loader 装载转换的顺序,默认配置为 'pre',也就是从右向左的顺序装载转换执行.如果想要设定转换时的顺序,可以配置为 'post',这样会按照从左向右的顺序装载转换执行.

### css-loader

> 问题1
  
  css-loader 使用参数minimize进行样式表压缩处理,webpack 报错,为什么?

  - 介绍.

    在 webpack css-loader 中参数使用 minimize 进行样式表压缩处理会报错.

        Module build failed (from ./node_modules/css-loader/dist/cjs.js):
        ValidationError: Invalid options object. CSS Loader has been initialized using an options object that does not match the API schema.
        - options has an unknown property 'minimize'. These properties are valid:
          object { url?, import?, modules?, sourceMap?, importLoaders?, esModule?, exportType? }

  - 原因.

    在 webpack 3.0 以及 css 1.0 以后,style-loader 不再支持参数 minimize 样式表压缩处理.

# webpack-dev-server 配置

> 原理

  webpack-dev-server 主要是运用了 websocket 长链接以及 webpack --watch 监听. webpack-dev-server 会将 websocket client 端注册至远程客户端中,当 --watch 监听到项目内容发生变化时,webpack 就会重新实行构建打包,websocket server 端通过长链接会告知 websocket client 端从而实行远程客户端页面的强制刷新.
  
  - hot.
  
    hot 是 webpack-dev-server 配置中的属性,意为是否开启模块热更新.实际上就是指当通过长链接会告知 websocket client 端从而实行远程客户端页面的强制刷新时,这时候不实行整个页面的全局刷新,而是实行发生局部改动模块的热替换,从而实现局部刷新.

## devServer

### contentBase

> 问题1

  webpack5 中 devServer 的属性 contentBase 被删掉了吗?

  - 介绍.

    在 webpack5 中配置 devServer contentBase 会报错. contentBase 意为配置 DevServer 服务的根目录,一般情况下 contentBase 为项目根目录.

        [webpack-cli] Invalid options object. Dev Server has been initialized using an options object that does not match the API schema.
        - options has an unknown property 'contentBase'. These properties are valid:
          object { allowedHosts?, bonjour?, client?, compress?, devMiddleware?, headers?, historyApiFallback?, host?, hot?, http2?, https?, ipc?, liveReload?, magicHtml?, onAfterSetupMiddleware?, onBeforeSetupMiddleware?, onListening?, open?, port?, proxy?, server?, setupExitSignals?, setupMiddlewares?, static?, watchFiles?, webSocketServer? }

  - 原因.

    据 issues <a href='https://github.com/webpack/webpack-dev-server/issues/2958#issuecomment-757141969'>Multiple bugs in one (config-yargs is needed and invalid configuration object errors) </a>,contentBase 属性已经在 webpack5 中更名为 static.

### open

> 介绍

  open 属性配置源于<a href='https://www.npmjs.com/package/open'>open 依赖库</a>,跨平台,可打开URL和可执行文件. 当在开发环境使用webpack-dev-server时,配置使用 open 属性,运行则可自动打开浏览器标签页展示配置的页面.

> 问题1

  open 属性配置可打开并复用浏览器同一个标签页吗?

  - 介绍

    每次在开发环境执行 webpack-dev-server 命令,自动打开一个新的浏览器标签页,原来的标签页还在,但是并没有复用.

        webpack-dev-server --config=./webpack.config.js --color

  - 进度

    据 issues <a href='https://github.com/webpack/webpack-dev-server/issues/1400'>Re-use current tab instead of open a new one</a>, 现阶段只能做到在 mac 平台上面做到打开并复用浏览器同一个标签页,而 facebook 的 create-react-app 框架也是一样 <a href='https://github.com/facebook/create-react-app/blob/25184c4e91ebabd16fe1cde3d8630830e4a36a01/packages/react-dev-utils/openBrowser.js#L65-L86'>create-react-app -> openBrowser.js</a>,目前还没有任何新的方案做到跨平台打开并复用浏览器同一个标签页.

## resolve

### mainFields

> 介绍

  根据 target 的不同,配置解析导入模块的字段也就不同. 默认配置解析导入模块的字段为: ['browser', 'module', 'main'],按照从左向右的顺序配置解析.

### modules

> 介绍

  通常解析导入模块时会直接通过 modules 默认配置 ['node_modules'],但如果想要直接通过个人所写的业务组件库解析导入模块,可以自定义 modules 配置,例如: [path.join(__dirname, 'src', 'components'), 'node_modules'],这样会优先解析导入个人所写的业务组件库,实行解析导入模块.

### descriptionFiles

> 介绍

  用于描述项目依赖的 JSON 文件,也可以自定义,默认配置为['package.json'],建议不要随意修改.
