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

   commonjs的特性就是其导入不是在编译器编译时执行的,而是在代码执行时才实行的.其特性导致了两个特点: 动态导入和赋值复制.

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

   优势: 导入比较灵活;NodeJS模块导入导出完全采用commonjs模式,npm上绝大部分的依赖库都会兼容commonjs模块导入导出,适用范围很广泛;同步模块加载;

   劣势: 不支持静态分析,静态分析所带来的一系列福利不能在commonjs模块导入导出模式下施行;不能实行异步模块加载;

> amd(cmd)

   amd(cmd)的适用范围很窄,受众面也远远没有commonjs和esm广泛,因为受限于第三方库的环境依赖(无论是SeaJS,还是RequireJS都需要事先下载依赖).

   * 优势和劣势.

   优势: 支持同步/异步模块加载,amd近似于同步模块导入导出(与commonjs同步模块加载有着本质的不同),cmd异步模块导入导出;

   劣势: 不支持静态分析,静态分析所带来的一系列福利()不能在amd(cmd)模块导入导出模式下施行;受限于第三方库的环境依赖;写法上很不友好;适用范围很窄,没有类NodeJS、npm以及ECMAScript标准这种受众面很广泛的'推手'推动;

> umd

   umd,全称: Universal Module Definition,其标准为: <a href='https://github.com/umdjs/umd/blob/master/templates/commonjsStrictGlobal.js'>commonjsStrictGlobal</a>以及<a href='https://github.com/umdjs/umd/blob/master/templates/returnExportsGlobal.js'>returnExportsGlobal</a>.

   * commonjsStrictGlobal.   

   ```javascript
   // Uses CommonJS, AMD or browser globals to create a module. This example
   // creates a global even when AMD is used. This is useful if you have some
   // scripts that are loaded by an AMD loader, but they still want access to
   // globals. If you do not need to export a global for the AMD case, see
   // commonjsStrict.js.
   
   // If you just want to support Node, or other CommonJS-like environments that
   // support module.exports, and you are not creating a module that has a
   // circular dependency, then see returnExportsGlobal.js instead. It will allow
   // you to export a function as the module value.
   
   // Defines a module "commonJsStrictGlobal" that depends another module called
   // "b". Note that the name of the module is implied by the file name. It is
   // best if the file name and the exported global have matching names.
   
   // If the 'b' module also uses this type of boilerplate, then
   // in the browser, it will create a global .b that is used below.
   
   (function (root, factory) {
      if (typeof define === 'function' && define.amd) {
         // AMD. Register as an anonymous module.
         define(['exports', 'b'], function (exports, b) {
            factory((root.commonJsStrictGlobal = exports), b);
         });
      } else if (typeof exports === 'object' && typeof exports.nodeName !== 'string') {
         // CommonJS
         factory(exports, require('b'));
      } else {
         // Browser globals
         factory((root.commonJsStrictGlobal = {}), root.b);
      }
   }(typeof self !== 'undefined' ? self : this, function (exports, b) {
      // Use b in some fashion.
   
      // attach properties to the exports object to define
      // the exported module properties.
      exports.action = function () {};
   }));
   ```

   * returnExportsGlobal.

   ```javascript
   	// Uses CommonJS, AMD or browser globals to create a module.

    // If you just want to support Node, or other CommonJS-like environments that
    // support module.exports, and you are not creating a module that has a
    // circular dependency, then see returnExports.js instead. It will allow
    // you to export a function as the module value.
   
    // Defines a module "commonJsStrict" that depends another module called "b".
    // Note that the name of the module is implied by the file name. It is best
    // if the file name and the exported global have matching names.
   
    // If the 'b' module also uses this type of boilerplate, then
    // in the browser, it will create a global .b that is used below.
   
    // If you do not want to support the browser global path, then you
    // can remove the `root` use and the passing `this` as the first arg to
    // the top function.
   
    (function (root, factory) {
       if (typeof define === 'function' && define.amd) {
          // AMD. Register as an anonymous module.
          define(['exports', 'b'], factory);
       } else if (typeof exports === 'object' && typeof exports.nodeName !== 'string') {
          // CommonJS
          factory(exports, require('b'));
       } else {
          // Browser globals
          factory((root.commonJsStrict = {}), root.b);
       }
    }(typeof self !== 'undefined' ? self : this, function (exports, b) {
       // Use b in some fashion.
    
       // attach properties to the exports object to define
       // the exported module properties.
       exports.action = function () {};
    }));
   ```

   从源码中可以看出umd是对于commonjs、Node(webpack中值枚举为commonjs2)、amd以及Browser globals的并集,是实行兼容的一种模块导入导出模式.

   * 优势和劣势.

   优势: 兼容的这几种模块导入导出都支持同步模块加载,导入比较灵活;导出的类型更丰富;
   
   劣势: 不支持静态分析,静态分析所带来的一系列福利也不能在umd模块导入导出模式下施行;不能实行异步模块加载;

> esm(ecmascript module)

   模块导入导出的最终方案模式.也是现在NodeJS、npm以及ECMAScript标准这些受众面很广泛的'推手'主要推动的模块导入导出模式.

   其导入是在编译器编译阶段,由此特性也导致了两个特点: 静态分析和赋值引用. 与commonjs的特性与特点完全相反.
   
   * NodeJS ESM.

   混用阶段,也就是import配合module.exports,require配合export. 
   
   注意在import配合module.export这部分,import必须导入module.exports导出的模块,不能导入exports导出的模块,由于还是以commonjs模块导出,那么esm导入就不能实行静态分析,只能整体导入.
   
   ```javascript
   //module.js
   const count = 4;
   //NodeJS ESM必须使用module.exports导出
   module.exports = {
     count	 
   };
   ```

   ```javascript
   //index.js
   //以commonjs模块导出,esm导入不能实行静态分析,只能整体导入.
   //import {count} from './module.js';
   import module from './module.js';
   console.log(module.count);
   ```

   * 静态分析.

   其导入是在编译器编译阶段,所以需要将所使用的模块都在所要导入文件的头部进行导入.可对其引入的值、函数或者模块可进行静态分析.

   ```javascript
   //module.js
   export const count = 4;
   ```

   ```javascript
   //index.js
   //对其引入的值、函数或者模块可进行静态分析
   import {count} from './module.js';
   console.log(count);
   ```

   * 赋值引用.

   ```javascript
   //module.js
   export let count = 4;
   export function add() {
     count++;
   }
   ```

   ```javascript
   //index.js
   import {count, add} from './module.js';
   console.log('count:', count);
   add();
   console.log('count:', count);
   ```

   上面这两段js代码与commonjs赋值复制部分是同一个例子,但是执行结果却是不相同的,其结果为:

    count: 4
    count: 5

   可以看出esm对于导出的变量以及函数都是编译器编译时连同引用一起导出,导致通过模块内部修改变量的值之后,在外部导入模块变量的值也发生了改变.

   * 优势和劣势.

   优势: 支持静态分析,静态分析所带来的一系列福利都可接收;可实行异步模块加载;支持动态导入import();
   
   劣势: 现阶段NodeJS、npm以及ECMAScript标准这些受众面很广泛的'推手'因历史、兼容、底层改动大等问题,实现的都不成熟,还需要Webpack/Babel等工具进行转译;