---
title: think of typescript
date: 2022-05-26 22:39:25
tags: [typescript, javascript module, webpack]
categories: typescript
---

# typescript配置

## {"module": "umd"}

> 问题1

  近期在配置typescript配合webpack构建打包项目时发现,tsconfig.json里配置模块导入导出模式为{"module":"umd"},webpack使用ts-loader来处理,导入导出的模块模式同样使用{libraryTarget: 'umd'} or output -> library: {type: 'umd'},构建打包出来的模块竟然不识别(下图就是模块不识别的异常)......反之,我配置为{"module":"commonjs"},webpack采用同样的配置,构建打包出来的模块就是可以识别的,并且运行很正常.

  - 奇事.

    - 标准"umd"中不是包含"commonjs"吗?
    - webpack为啥在模块导入导出模式中会出现只识别typescript {"module":"commonjs"},而不识别typescript {"module":"umd"}呢？

  - 异常.

  ![](/images/typescript_module_umd.png)

  - 原因.

    其根本原因,是typescript的对于模块导入导出的"umd"模式并没有遵从统一的"umd"标准来.

    - "umd"标准.
    
      <a href='https://white-than-wood.github.io/2022/05/23/thinkofjsmodule/#%E6%A8%A1%E5%9D%97%E5%AF%BC%E5%85%A5%E5%AF%BC%E5%87%BA%E7%9A%84%E5%8E%86%E5%8F%B2'>think of JsModule -> 模块导入导出的历史 -> "umd"部分</a>
    
    - typescript "umd".
    
      ```javascript
      (function (factory) {
        if (typeof module === 'object' && typeof module.exports === 'object') {
           var v = factory(require, exports); if (v !== undefined) module.exports = v;
        } else if (typeof define === 'function' && define.amd) {
           define(["require", "exports"], factory);
        }
      })(function (require, exports) {});
      ```

  - 结论.

    通过两套"umd"配置可以看出,没有标准"umd"条件中的第三种处理,允许导出到window.namesapace = export;因此,当大量开发人员需要同时支持所有这三个时,当前的typescript {"module":"umd"}模块导入导出机制是非常糟糕的,webpack也是基于typescript {"module":"umd"}产生的这种问题,对typescript {"module":"umd"}模块导入导出模式是不支持的,所以会出现只识别typescript {"module":"commonjs"},不识别typescript {"module":"umd"}的奇事发生.

