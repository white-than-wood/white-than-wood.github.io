---
title: think of webpack
date: 2022-05-23 00:46:01
tags: [webpack,javascript module]
categories: webpack
---

# webpack 配置

## output

> 问题1

   近期在配置 webpack 时,发现 output -> libraryType: 'umd' or output -> library: {type: 'umd'} 打包构建导出的模块无法实行 TreeShaking.

> 介绍

   在配置 webpack 的输出 output 时,都会遇到输出模块的配置,也就是 webpack4 中的 libraryType 以及 webpack5 中的 libraryType(未删但新特性不更新) and library: {type: 'xxx'},首先解释一下这几个属性的含义,libraryType 指的是每个 chunk 模块以怎样的模块形式构建打包输出,而 library: {type: 'xxx'} 在webpack5 中属于对 libraryType 的复写,跟 libraryType 是一个含义.
   
   * libraryType 的值枚举

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

   * libraryType 中包含模块导入导出的值枚举

   'commonjs' | 'commonjs2' | 'amd' | 'umd' | 'module' 这五个值枚举代表了模块导入导出历史的发展轨迹,由最初的 commonjs 到最终方案 esm,关于它们的含义以及对比,这里把它放在了<a href='https://white-than-wood.github.io/2022/05/23/thinkofjsmodule/#%E6%A8%A1%E5%9D%97%E5%AF%BC%E5%85%A5%E5%AF%BC%E5%87%BA%E7%9A%84%E5%8E%86%E5%8F%B2'>think of JsModule#模块导入导出的历史</a>中,这部分非常重要,是为后续下文的理解做铺垫.
   
   * TreeShaking

   TreeShaking 最初是 <a href='https://rollupjs.org/guide/en/'>RollUp</a> 团队推出的一个方案,以 esm 模块导入导出模式的静态分析为依赖基础,在编译时对项目代码进行瘦身,将无用、空引入的代码模块不构建进打出的包中,减少代码体积,减少项目冗余,提高项目运行速度的模式.
   
   * import umd

   对于 umd 的 esm import 导入,在 commonjs、NodeJS 环境下都可以实现,但是必须注意 <a href='https://white-than-wood.github.io/2022/05/23/thinkofjsmodule/#%E6%A8%A1%E5%9D%97%E5%AF%BC%E5%85%A5%E5%AF%BC%E5%87%BA%E7%9A%84%E5%8E%86%E5%8F%B2'>esm(ecmascript module)</a> NodeJS ESM部分.

> 结论

   由于上述 umd 所兼容的所有模块导入导出模式不能实现静态分析,包括 import 配合 module.exports (使用 esm import 导入 commonjs2 值枚举导出的模块),而 TreeShaking 又是以 esm 模块导入导出模式的静态分析为依赖基础的,所以 umd 模块导入导出模式不能实行 TreeShaking.