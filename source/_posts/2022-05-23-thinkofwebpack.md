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

## module

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

    在 webpack5 中配置 devServer contentBase 会报错.

        [webpack-cli] Invalid options object. Dev Server has been initialized using an options object that does not match the API schema.
        - options has an unknown property 'contentBase'. These properties are valid:
          object { allowedHosts?, bonjour?, client?, compress?, devMiddleware?, headers?, historyApiFallback?, host?, hot?, http2?, https?, ipc?, liveReload?, magicHtml?, onAfterSetupMiddleware?, onBeforeSetupMiddleware?, onListening?, open?, port?, proxy?, server?, setupExitSignals?, setupMiddlewares?, static?, watchFiles?, webSocketServer? }

  - 原因.

    据 issues <a href='https://github.com/webpack/webpack-dev-server/issues/2958#issuecomment-757141969'>Multiple bugs in one (config-yargs is needed and invalid configuration object errors) </a>,contentBase 属性已经在 webpack5 中更名为 static.
