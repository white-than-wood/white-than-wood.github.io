---
title: think of babel
date: 2022-06-06 18:25:07
tags: [webpack,babel,commonjs,esm,amd,cmd,umd,javascript module]
categories: babel
---

# babel 配置

## @babel/perset-env

### useBuiltIns

> 介绍

  useBuiltIns是用于配合core-js,兼容旧版本浏览器原生不支持的新功能.枚举的值为: 'entry', 'usage' and false.
  
  - 'entry'

    - 配合 corejs: 2 时,会在入口直接导入全部的兼容代码.
    - 配合 corejs: 3 时,会在入口支持按需导入兼容代码.
  
  - false

    - 在任何位置不导入任何兼容代码.

  - 'usage'

    - 配合 corejs: 2时, 会在需要兼容的模块内导入全部的兼容代码.
    - 配合 corejs: 3时, 根据使用时的旧版本浏览器原生不支持的新功能按需在需要兼容的模块内进行导入.
    