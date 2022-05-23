---
title: think of webpack
date: 2022-05-23 00:46:01
tags: [webpack,javascript module]
categories: webpack
---

# webpack配置

> output -> libraryType: 'umd' or output -> library: {type: 'umd'}打包构建导出的模块无法进行TreeShaking.

   在配置webpack的输出output时,都会遇到输出模块的配置,也就是webpack 4中的libraryType以及webpack 5中的libraryType(未删但新特性不更新) and library: {type: 'xxx'},首先解释一下这几个属性的含义,libraryType指的是每个chunk模块以怎样的模块形式构建打包输出,而library: {type: 'xxx'}在webpack 5中属于对libraryType的复写,跟libraryType是一个含义.
   
   * libraryType的值枚举

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

   * libraryType中包含模块导入导出的值枚举

   'commonjs' | 'commonjs2' | 'amd' | 'umd' | 'module' 这五个值枚举代表了模块导入导出历史的发展轨迹,由最初的commonjs到最终方案esm,关于它们的含义以及对比,这里把它放在了<a href=''>think of JsModule#模块导入导出的历史</a>中,这部分非常重要,是为后续下文的理解做铺垫.
   
   * TreeShaking