---
title: think of JsModule
date: 2022-05-23 00:48:24
tags: [commonjs,esm,amd,cmd,umd,javascript module]
categories: javascript module
---

# 模块导入导出的历史

> JsModule的演化经历

   下面这张图可以清晰的看出,javascript module演化的历史,由最初的commonjs到最终方案esm,而现在正处于umd -> esm阶段.

   ![](/images/js_module_history.png)

> commonjs

   commonjs的特性就是其不是在编译器编译时执行的,而是在代码执行时才实行的.其特性导致了其两个特点: 动态导入和赋值复制.

   * 动态导入.

   下面这段js代码完美诠释了此含义,在此判断为true的情况下的commonjs,才会导入selectivizr,并实行selectivizr中的脚本.

   ```javascript
    if(browser.desktop && browser.msie && browser.versionNumber < 9){
      require('selectivizr');
    }
   ```

   * 赋值复制.

   ```javascript
   // module.js
   let count = 4;
   function add() {
     count++;
   }
   module.exports = {
     count,
     add
   };	 
   ```

   ```javascript
   // index.js
   const {count, add} = require('./module.js');
   console.log('count:', count);
   add();
   console.log('count:', count);	 
   ```

   上面这两段js代码完美诠释了此含义,其结果为:

    count: 4
    count: 4

   可以看出commonjs对于导出的变量以及函数都是代码执行时直接复制其值,而不是连同引用一起导出,导致通过模块内部修改变量的值之后,在外部导入模块变量并没有发现其值发生变化.

   * 优势和劣势.

   优势: 导入比较灵活;NodeJS模块导入导出完全采用commonjs模式;npm上绝大部分的依赖库都会兼容commonjs模块导入导出;同步模块加载;
   劣势: 不支持静态分析,静态分析所带来的一系列福利不能commonjs模块导入导出模式下施行;不能实行异步加载;

> amd(cmd)

> umd

> esm(ecmascript module)