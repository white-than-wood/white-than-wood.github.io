---
title: think of postcss
date: 2022-05-25 19:11:27
tags: [postcss, webpack]
categories: postcss
---

# postcss配置

> postcss在概念上主要是为了使新的css样式以及特性在更多种类的浏览器上得到兼容

  - css新样式.

  ```css
  /*经过postcss转译之前*/
  .dom {
    user-select: none;  
  }
  ```

  ```css
  /*经过postcss转译之后*/
  .dom {
      -webkit-user-select:none;
      -moz-user-select:none;
      -ms-user-select:none;
      user-select:none 
  }
  ```

  - css新特性.

  ```css
  /*经过postcss转译之前*/
  .dom {
    color: #12345678;  
  }
  ```

  ```css
  /*经过postcss转译之前*/
  .dom {
    color: rgba(18,52,86,.471);
  }
  ```

> 现阶段postcss转译的工具使用率比较多的有两种: autoprefixer以及postcss-preset-env

  - autoprefixer.

    autoprefixer是将css的新样式在更多种类的浏览器中兼容.

    - 步骤.
    
      首先下载npm依赖包postcss-loader和autoprefixer.
    
          npm install autoprefixer postcss-loader -D
    
      之后创建postcss.config.js文件,配置使用插件.
    
          touch postcss.config.js
    
      ```javascript
      module.exports = {
        plugins: [
          require('autoprefixer')
        ]
      };
      ```
      
      配置webpack构建打包postcss-loader.

      ```javascript
      module.exports = {
        //...
        module: {
          rules: [{
            test: /.css$/,
            use: [{
              loader: 'style-loader'
            }, {
              loader: 'css-loader',
              importLoaders: 1
            }, {
              loader: 'postcss-loader'
            }]
          }]			
        }
        //...
      };
      ```

      当然也可以直接不创建postcss.config.js文件,在webpack构建打包postcss-loader配置.

      ```javascript
      module.exports = {
        //...
        module: {
          rules: [{
            test: /.css$/,
            use: [{
              loader: 'style-loader'
            }, {
              loader: 'css-loader',
              importLoaders: 1
            }, {
              loader: 'postcss-loader',
              options: {
                postcssOptions: {
                  plugins: [
                    require('autoprefixer')
                  ]
                }
              }
            }]
          }]			
        }
        //...
      };
      ```
      
      配置package.json -> browserslist属性,也就是postcss这种扩展兼容操作要选择适用哪些种类浏览器的版本,这一部分我们会拿出来在后续部分细讲.
    
    - 缺点.

      只是对css的新样式在更多种类的浏览器上做兼容,新特性被排除在外,所以autoprefixer所做出的扩展兼容是不完整的.
    
  - postcss-preset-env.

    postcss-preset-env是将css的新样式以及新特性在更多种类的浏览器中兼容,内置autoprefixer,含有autoprefixer的功能.

      - 步骤.

        首先下载npm依赖包postcss-preset-env和postcss-loader.

            npm install postcss-preset-env postcss-loader -D

        之后创建postcss.config.js文件,配置使用插件.

            touch postcss.config.js

        ```javascript
        module.exports = {
          plugins: [
            require('postcss-preset-env')
          ]
        };
        ```

        配置webpack构建打包postcss-loader.

        ```javascript
        module.exports = {
          //...
          module: {
            rules: [{
              test: /.css$/,
              use: [{
                loader: 'style-loader'
              }, {
                loader: 'css-loader',
                importLoaders: 1
              }, {
                loader: 'postcss-loader'
              }]
            }]			
          }
          //...
        };
        ```

        当然也可以直接不创建postcss.config.js文件,在webpack构建打包postcss-loader配置.

        ```javascript
        module.exports = {
          //...
          module: {
            rules: [{
              test: /.css$/,
              use: [{
                loader: 'style-loader'
              }, {
                loader: 'css-loader',
                importLoaders: 1
              }, {
                loader: 'postcss-loader',
                options: {
                  postcssOptions: {
                    plugins: [
                      require('postcss-preset-env')
                    ]
                  }
                }
              }]
            }]			
          }
          //...
        };
        ```
        
        配置package.json -> browserslist属性,也就是postcss这种扩展兼容操作要选择适用哪些种类浏览器的版本,这一部分我们会拿出来在后续部分细讲.

> browserslist

  postcss对css新样式以及新特性要选择适用哪些种类浏览器的版本所对应的属性.推荐在package.json文件里直接进行配置.

  - 默认配置.

    默认配置["defaults"]实际上的含义是["> 2%","last 2 versions","not dead"],是这三种筛选适用浏览器版本的方式的并集,这三种方式具体的含义在下一部分会具体介绍.
  
  ```json
  {
    "browserslist": [
      "defaults"
    ]
  }
  ```

  - 筛选适用浏览器版本方式枚举.

    - \["> n%"\]: 选择用户比例大于n%的所有种类的浏览器版本.
    - \["last n versions"\]: 选择所有种类的浏览器的最近n个版本.
    - \["not dead"\]: 选择官方并没有舍弃删除掉的所有种类的浏览器版本.
    
    所以["defaults"]的含义就是选择用户比例大于2%的、官方并没有舍弃删除掉的、所有种类的浏览器的最近2个版本的并集.

  - 其他配置.

    - \["> 2% and last 2 versions and not dead"\]: and表示"且"也就是交集,此🌰的含义为选择用户比例大于2%的、官方并没有舍弃删除掉的、所有种类的浏览器的最近2个版本的交集.
    - \["> 2% or last 2 versions or not dead"\]: 除了","之外,"or"也可以表示交集.跟["defaults"]的含义相同.
    - \["not > 2%"\]: "not"代表"非"也就是非集,此🌰的含义为选择用户比例小于等于2%的所有种类的浏览器的版本.

    