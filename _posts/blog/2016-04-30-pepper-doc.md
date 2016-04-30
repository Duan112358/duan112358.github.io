---
layout: post
title: pepper doc
category: blog
description: 都说太复杂了，难道真是这个样子吗
---

[![NPM Version](http://img.shields.io/npm/v/we-pepper.svg?style=flat)](https://www.npmjs.org/package/we-pepper)
[![NPM Downloads](https://img.shields.io/npm/dm/we-pepper.svg?style=flat)](https://www.npmjs.org/package/we-pepper)

-  [使用说明](#starter)
-  [工程配置](#configuration)
-  [目录结构](#directory)
-  [注意事项](#notices)
-  [使用文档](#documention)
-  [更新历史](#versions)
-  [TODOS](#todos)
-  [问题列表](#issues)

### Start

*  环境依赖
`nodejs` , `npm`

*  Pepper 安装  
   - npm 全局安装  

      ```
      npm install we-pepper -g # install pepper globally
      ```
*  项目初始化

    ```
    pepper init [project_name] # create a starter project
    ```
    执行该命令会创建一个 `project_name` 项目，结构如下
    
    ```
    .
    ├── /dist/                              # 代码打包目录
    ├── /node_modules/                      # node依赖包
    ├── /src/                               # 源码目录
    │   ├── /pages/                         # 页面
    │   ├── /components/                    # 公用组件
    │   ├── /assets/                        # 图片、字体资源
    │   ├── /scss/                          # 公用样式    
    │   └── /utils/                         # 公用JS模块
    ├── pepper.config.js                    # pepper的配置文件
    ├── index.template.html                 # 首页HTML模版, 可选
    └── package.json                        # 这个就不用说了吧
    ```

*  开发调试

   ```
   pepper start
   
   # or

   pepper start -p PORT

   ```
   该命令会启动一个 `express` 服务，负责托管所有的静态资源，支持 Code Hot Load，支持 Mock 服务，支持路由和数据格式的配置。
  
   **注意**：该模式下，静态资源都缓存在内存中，配置的`build`目录里是看不到静态资源的

*  创建页面  

    ```
    pepper page page1 # for example
    
    ├── /src/  
    │   ├── /components/
        ├── /pages/
        │   ├── /page1/    
        │       ├── index.jsx 
                ├── style.scss    
    ```
    
*  创建组件  

    ```
    pepper component Header # create Component Header
    ```
    
    ```
    . 
    ├── src
        ├── assets
        │   └── images
        │       └── favicon.ico
        ├── components
        │   ├── Header
        │       ├── header.scss
        │       ├── index.jsx
        │       └── style.scss
        ├── pages
    ```
    命令完成后，还需要手动编辑下`components/index.js`，将新创建的组件导出。
     
    ```
    ...
    import Header from './Header'
    
    export default {
        ...
        Header,
    }
    ```

*  资源打包  
    
    ```
    pepper test; // build in test mode, not enable JS&CSS Minify
    
    pepper pre;  // build in pre mode, enable JS&CSS Minify
    
    pepper release;// build in release mode, enable JS&CSS Minify
    
    ```
    需要注意的是，上述模式都会在配置的`build`目录生成静态资源，但都不启动静态资源的托管服务。
        
    `test`模式不启用代码压缩混淆，`pre`和`release`都会进行压缩和混淆，两者的区别在于`pepper.config.js`中的配置的URL。
    
*  流程引导  
   
   命令行下直接输入 `pepper`

   ```
  ? 选择要执行的任务:  (Use arrow keys)
  ❯ 项目打包
    开发调试
    初始化新项目 (es5)
    初始化新项目 (es6 & react & react-router)
    初始化新项目 (es6 & react & react-router & redux)
    创建新页面
    创建新组件
    升级pepper

   ```
   然后选择你要执行的任务就可以了

### Configuration  

```
{
    "host": "0.0.0.0",
    "port": "8080",
    "base": "src",
    "build": "dist",
    "static": { // CDN domain, or just leave it blank if not using
        "start": "/",
        "test": "/",
        "pre": "//static.wepiao.com/",
        "release": "//static.wepiao.com/"
    },
    "globals": {  // 配置全局变量，这里配置了 static_api 和 api 两个全局变量
        // static api
        "static_api": {
            "start": "", // local api base entry
            "test": "//m.wepiao.com",
            "pre": "//m.wepiao.com", // online api base entry
            "release": "//m.wepiao.com"
        },
        "api": {
          "start": "",
          "test": "",
          "pre": "//api.wepiao.com",
          "release": "//api.wepiao.com"
      }
    },
    "vendor": ["react", "react-dom"],
    "alias": {
        "wepiao": "components",
        "scss": "scss"
    },
    "devtool": "source-map",
    "template": {
        "path": "index.template.html", //使用自定义模版，忽略下面所有设置
        "title": "",
        "keywords": "",
        "description": "",
        "viewport": "",
        "favicon": "assets/images/favicon.ico"
    },
    "pages": "pages",
    "scss": "scss",
    "components": "components",
    "assets": "assets",
    "eslint": false, // 如果启用该选项，请在项目根目录提供自己的 `.eslintrc`
    "css_modules": false,
    "transfer_assets": false,
    "base64_image_limit": 10240,
    "esmode": 'es6' // swith esmode on template creating, es5/es6, default to es6
}
```
**注意**：路径的配置建议使用 `//`开头，省去协议名字 `http:`，这主要是为了向 https 迁移，因为`//`资源，会使用和当前网页相同的协议加载资源。

程序有四种模式:
*  start                开发调试, 静态资源内存缓存, 启express服务，可自定义端口和host，hot reload
*  test                 测试模式，不启服务，不压缩 
*  pre                  预发布，不启服务，压缩
*  release              发布模式, 不启服务，压缩




  key                  | 说明                                                                                                       
  ----                 |----------
  host                 | 调试IP, 默认0.0.0.0                                                                                        
  port                 | 调试端口, 默认8080                                                                                         
  base                 | 业务代码根目录                                                                                             
  build                | 代码打包目录                                                                                               
  static               | 静态资源跟路径，便于切换到CDN                                                                              
  api                  | 接口根路径                                                                                                 
  vendor               | 打包类库资源                                                                                               
  alias                | 路径别名, 添加后，在任意子目录引用: `import { some_thing } from 'alias_name/..'`                           
  devtool              | webpack调试模式设置, 具体参见: [webpack-devtool](http://webpack.github.io/docs/configuration.html#devtool) 
  pages                | 自定义 pages 路径                                                                                          
  scss                 | 自定义 scss 路径                                                                                           
  assets               | 自定义 assets 路径                                                                                         
  components           | 自定义 components 路径                                                                                     
  eslint               | 是否开启 eslint，默认关闭, 如果启用该选项，请在项目根目录提供自己的 `.eslintrc`                            
  css_modules          | 是否启用 [CSS Modules](https://github.com/css-modules/css-modules), 默认关闭                               
  esmode               | 模版代码风格，支持 `es5` 和 `es6` , 默认为 `es6`, 兼容 `es7`                                               
  transfer_assets      | 打包时是否 copy `asset` 目录，默认 false                                                                   
  base64_image_limit   | 对所有小于该大小的图片启用 base64 转码, 默认 10240 (10k)                                                   
  template.title:      | 模版 `document.title`                                                                                      
  template.keywords    | 模版 `meta keywords`, 默认为空                                                                             
  template.description | 模版 `meta description`, 默认为空                                                                          
  template.viewport    | 模版 `meta viewport`, 默认`width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no`         
  template.favicon     | 模版 `favicon`, 默认为空                                                                                   
  template.path:       | 设定自定义模版的的路径，会替换默认模版,可选。设置后会忽略上面五项模版设置. 有语法要求, 参考                

##### template.html

 <pre>
 <code>
   <!DOCTYPE html>
   <html>
     <head>
       <meta charset="utf-8">
       <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
       <meta name="keywords" content="webpack, react, es6, build tools"/>
       <meta name="description" content="webpack auto build tools intergated with one command"/>
       <meta name="apple-mobile-web-app-capable" content="yes"/>
       <meta name="format-detection" content="telephone=no"/>
       {% if (o.htmlWebpackPlugin.files.favicon) { %}
       <link rel="shortcut icon" href="{%=o.htmlWebpackPlugin.files.favicon%}">
       {% } %}
       {% for (var css in o.htmlWebpackPlugin.files.css) { %}
       <link href="{%=o.htmlWebpackPlugin.files.css[css] %}" rel="stylesheet">
       {% } %}
       {% if(o.chunkManifest) { %}
       <script>{%#o.chunkManifest%};</script>
       {% } %}
       <title>frontend-build-tools</title>
     </head>
     <body>
         <div id="app">
         </div>
       {% for (var chunk in o.htmlWebpackPlugin.files.chunks) { %}
       <script type="text/javascript" src="{%=o.htmlWebpackPlugin.files.chunks[chunk].entry%}"></script>
       {% } %}
     </body>
   </html>
  </code>
  </pre>

**API Mock && API Proxy**  
   
   修改`mock.js`，配置如下  

   ```
    var config =  [{
        path: /\/list/,   // request url
        method: 'get',    // request method
        data: [{          // response data
            id: 1,
            first: '@FIRST',
        }, {
            id: 2,
            first: '@FIRST',
        }, {
            id: 3,
            first: '@FIRST',
        }, ]
    }, {
        path: /\/item/,
        method: 'get',
        data: function() {
            return {
                id: 1,
                name: 'inf'
            }
        }
    }, {  // example proxy config
        path: '/path_to_proxy',
        proxy: 'http://example.com',         // http://example.com
        changeOrigin: true,              // default true
        pathRewrite: {                   // omit it if not change  
            '^/old/path': '/new/path'
        },
        ws: true // switch for websocket // default true
    }];

    // export your mock config
    module.exports = config;

   ```
   `Pepper`会读取该文件，将上述配置加入启动的express服务中，更多数据格式参见[mockjs](http://mockjs.com)

### Directory

```
.
├── /dist/                              # The folder for compiled output
├── /node_modules/                      # 3rd-party libraries and utilities
├── /src/                               # The source code of the application
│   ├── /pages/                         # Pages entry
│   ├── /components/                    # React components
│   ├── /assets/                        # Static files which are copied into the /dist/assets folder
│   ├── /scss/                          # Shared scss files, will compile into /dist/css/share.css
│   └── /utils/                         # Utility classes and functions
├── pepper.config.js                    # project config setting
├── index.template.html                 # Index page template, 可选
└── package.json                        # The list of 3rd party libraries and utilities
```

### Notices
  -  API  
     `pepper.config.js`中配置的`api`在代码中将以全局变量'API'的形式导出  
     
     ```
     console.log(API) // pepper.config.js中的api[MODE]设置,注意使用环境
     Reqwest({
         url: API + '/your_sub_url',
         ...
     })
     ```
  -  template
     html模版可选，如果不提供，请在`pepper.config.js`的`template`中指定以下选项 
      
     ```
     title: '',
     keywords: '',
     description: '',
     viewport: '',//可选
     favicon: ''
     ```
     如果在`pepper.config.js`中指定了'template.path'选项, `pepper`将采用指定的HTML模版，注意要满足上文的格式要求  
     另外，`pepper`支持`jade`语法

  -  **chunkhash**
    [ Vendor chunkhash changes when app code changes ](https://github.com/webpack/webpack/issues/1315) remain
    
    ```
    # test toturial
    pepper init demo;
    cd demo;
    pepper pre;
    #  remove about page entry in pages/index.jsx
    pepper pre;
    ```
    ```
    ➜  demo  pepper pre
    Hash: bfbf7a48f33bf6a9e709
    Version: webpack 1.12.11
    Time: 9253ms
                     Asset       Size  Chunks             Chunk Names
     js/shared-e9219f0d.js      35 kB    0, 3  [emitted]  shared
       js/home-646aa985.js    9.05 kB    1, 3  [emitted]  home
     js/vendor-5a9e0e26.js     131 kB    2, 3  [emitted]  vendor
                index.html  648 bytes          [emitted]
       [0] multi vendor 40 bytes {2} [built]
        + 199 hidden modules
    ➜  demo  vim src/pages/index.jsx
    ➜  demo  pepper pre
    Hash: d72ff25390f520660578
    Version: webpack 1.12.11
    Time: 8205ms
                    Asset       Size  Chunks             Chunk Names
    js/shared-9f5500d8.js    34.7 kB    0, 3  [emitted]  shared
      js/home-1fb4ec5e.js    6.85 kB    1, 3  [emitted]  home
    js/vendor-5a9e0e26.js     131 kB    2, 3  [emitted]  vendor
               index.html  647 bytes          [emitted]
       [0] multi vendor 40 bytes {2} [built]
        + 194 hidden modules
    ➜  demo
    ```