---
title: think of webpack
date: 2022-05-23 00:46:01
tags: [webpack,javascript module]
categories: webpack
---

# webpack配置

> output -> libraryType: 'umd' or output -> library: {type: 'umd'}打包构建导出的模块无法实行TreeShaking.

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

   'commonjs' | 'commonjs2' | 'amd' | 'umd' | 'module' 这五个值枚举代表了模块导入导出历史的发展轨迹,由最初的commonjs到最终方案esm,关于它们的含义以及对比,这里把它放在了<a href='https://white-than-wood.github.io/2022/05/23/thinkofjsmodule/#%E6%A8%A1%E5%9D%97%E5%AF%BC%E5%85%A5%E5%AF%BC%E5%87%BA%E7%9A%84%E5%8E%86%E5%8F%B2'>think of JsModule#模块导入导出的历史</a>中,这部分非常重要,是为后续下文的理解做铺垫.
   
   * TreeShaking

   TreeShaking最初是<a href='https://rollupjs.org/guide/en/'>RollUp</a>团队推出的一个方案,以esm模块导入导出模式的静态分析为依赖基础,在编译时对项目代码进行瘦身,将无用、空引入的代码模块不构建进打出的包中,减少代码体积,减少项目冗余,提高项目运行速度的模式.
   
   * import umd

   对于umd的esm import导入,在commonjs、NodeJS环境下都可以实现,但是必须注意 <a href='https://white-than-wood.github.io/2022/05/23/thinkofjsmodule/#%E6%A8%A1%E5%9D%97%E5%AF%BC%E5%85%A5%E5%AF%BC%E5%87%BA%E7%9A%84%E5%8E%86%E5%8F%B2'>esm(ecmascript module)</a> NodeJS ESM部分.
   
   由于上述umd所兼容的所有模块导入导出模式不能实现静态分析,包括import配合module.exports(使用esm import导入commonjs2值枚举导出的模块),而TreeShaking又是以esm模块导入导出模式的静态分析为依赖基础的,所以umd模块导入导出模式不能实行TreeShaking.
   
   * 参考文献:

   1. https://webpack.docschina.org/configuration/output/#outputlibrarytype
   2. https://webpack.docschina.org/guides/tree-shaking/#root
   3. https://www.ruanyifeng.com/blog/2020/08/how-nodejs-use-es6-module.html 