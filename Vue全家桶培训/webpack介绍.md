# webpack介绍
## 什么是webpack
webpack是**前端打包工具**，能让浏览器支持模块化，自动分析项目的所有依赖关系，然后按照指定的规则生产对应的静态资源。
官网在这：https://www.webpackjs.com/

+ webpack核心主要进行 JavaScript 资源打包
+ 如下图，它可以结合其他插件工具，将多种静态资源css、png、sass 分类转换成一个个静态文件，这样可以减少页面的请求。
+ 可集成 babel 工具实现 EcmaScript 6 转 EcmaScript 5 ，解决兼容性问题
+ 可集成 http 服务器
+ 可集成模块热加载，当代码改变后自动刷新浏览器 等等功能
![输入图片说明](https://images.gitee.com/uploads/images/2019/1225/120053_0951d025_5449551.png "屏幕截图.png")
## 全局安装webpack
```cmd
yarn global add webpack --dev
```
## 局部安装webpack
```cmd
yarn add webpack --dev
```
## 全局安装和局部安装webpack的对比
### 全局安装
+ 优点
全局安装的优点在于不需要每个项目都安装webpack，方便自己开发打包
+ 缺点
当自己的项目代码要给其他人使用时，要求他人必须安装与自己同样版本的webpack才能正常打包
### 局部安装
+ 优点
webpack与项目一起，不管项目代码在哪里使用都能保证webpack版本与自己一致，打包不受影响
+ 缺点
自己开发的时候，每个项目都要安装webpack
> 你们会选择哪种安装方式呢？

## webpack-cli
如果是webpack4以上版本，还需要安装webpack-cli才能使用webpack命令。
安装webpack-cli的方式与webpack的安装方式要保持一致，即webpack是全局安装的，webpack-cli也要全局安装。

```cmd
yarn global add webpack-cli --dev 或 yarn add webpack-cli --dev
```
安装成功后可以查看版本
+ 全局安装
```cmd
webpack -v
> 4.41.4
```
+ 局部安装
```cmd
项目路径\node_modules\.bin\webpack -v
> 4.41.4
```
> 由于局部安装必须要找到.bin目录才能执行webpack命令，每次输入比较麻烦，yarn提供简写方式
> yarn webpack -v
> 可以通过一条命令一并安装这两个库，用空格隔开，-D跟--dev一个意思
>  yarn add webpack webpack-cli -D

## webpack的小例子
### 创建webpack-demo项目
在vscode工作空间创建webpack-demo目录，在webpack-demo目录下执行命令
```cmd
yarn init
```
目录结构如下
![输入图片说明](https://images.gitee.com/uploads/images/2019/1225/140351_8f9262ab_5449551.png "屏幕截图.png")

bar.js代码如下
```js
// nodejs模块化编程，导出一个函数
module.exports = function(){
    console.log('我是bar模块')
}
```
在main.js中引用bar模块
```js
const bar = require('./bar')

bar()
```
index.html中引入main.js
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    
</body>
<script src="./js/main.js"></script>
</html>
```
浏览器访问index.html
![输入图片说明](https://images.gitee.com/uploads/images/2019/1225/140738_672c42c5_5449551.png "屏幕截图.png")

> 这里报错的原因是浏览器并不支持模块化编程，如果我们在项目目录下用node命令来执行main.js是可以运行的

```cmd
node .\js\main.js
```
<img src="https://images.gitee.com/uploads/images/2019/1225/141038_62fa2144_5449551.png" alt="输入图片说明" title="屏幕截图.png" style="zoom:200%;" />

通过webpack命令打包后看看，在项目目录下执行命令
```cmd
yarn webpack .\js\main.js -o .\js\bundle.js
```
> 第一个参数是源文件，第二个参数 **-o** 是输出的意思，第三个参数是输出文件

打包完成后js目录下会多一个bundle.js文件
![输入图片说明](https://images.gitee.com/uploads/images/2019/1225/142047_c077a2d9_5449551.png "屏幕截图.png")

index.html引入bundle.js文件，刷新页面，F12可以正常看到输出信息
```html
<script src="./js/bundle.js"></script>
```
![输入图片说明](https://images.gitee.com/uploads/images/2019/1225/142313_00074ca3_5449551.png "屏幕截图.png")
### 模块化目录结构规范
源代码放在src目录，打包后的文件放在dist目录
![输入图片说明](https://images.gitee.com/uploads/images/2019/1225/144022_40e84f4b_5449551.png "屏幕截图.png")

## 打包配置文件webpack.config.js
如果修改完js代码都要打包，每次打包都要指明源文件和输出文件比较麻烦，可以用webpack.config.js来解决。
### 创建webpack.config.js
在项目目录下创建webpack.config.js
![输入图片说明](https://images.gitee.com/uploads/images/2019/1225/144114_c70331fa_5449551.png "屏幕截图.png")

webpack.config.js的配置代码如下
```js
// 引入nodejs路径小工具
const path = require('path')

// 导出webpack配置对象
module.exports = {
    // 入口js
    entry: './src/main.js',

    // 出口配置对象
    output: {
        // path必须是绝对路径，所以这里需要引入nodejs的路径小工具来帮助我们找到当前目录的绝对路径
        // __dirname是path内置的全局变量，当前js文件所在目录的绝对路径
        path : path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    // 打包模式有三种，development、production、none
    mode: 'development'
}
```
**打包模式说明**
+ development 开发模式，输出的bundle.js文件不会被压缩
+ production 生产模式，**默认值**，输出的bundle.js会被压缩
+ none 没有模式，目测与development效果一样

再执行一次webpack命令，这次不需要任何参数
```cmd
yarn webpack
```
## 命令别名
命令别名可以简化命令的输入，或者以自己容易理解的词语代替默认命令。
### 修改package.json
添加scripts属性
```json
{
  "name": "webpack-demo",
  "version": "1.0.0",
  "description": "学习webpack用的",
  "main": "index.js",
  "author": "pjh",
  "license": "MIT",
  "devDependencies": {
    "webpack": "^4.41.4",
    "webpack-cli": "^3.3.10"
  },
  "scripts": {
    "show": "webpack -v",
    "start": "node ./src/main",
    "build": "webpack"
  }
}
```
尝试输入自定义命令
```cmd
yarn show
yarn start
yarn build
```
## 模块化编程的ES6规范
| ES5 | ES6 | 说明 |
| --- | --- | --- |
| require() | import | 导入模块 |
| module.exports | export | 导出成模块 |
| 无 | export default | 导出默认成员，只能有一个 |
### 修改webpack-demo项目
**bar.js**
```js
export default function () {
    console.log('这是bar模块')
}
```
**main.js**
```js
import bar from './bar'

bar()
```
### 导出多个变量
**bar.js**
```js
export const a = 1
export const b = 2

export function sum() {
    return a + b
}

export function decrease() {
    return a - b
}
```
**main.js**
+ 导入部分变量

```js
import {a,b,sum} from './bar'

console.log(a)
console.log(b)
console.log(sum())
```
+ 导入所有变量
```js
import * as bar from './bar'

console.log(bar.a)
console.log(bar.b)
console.log(bar.sum())
console.log(bar.decrease())
```
> 只**export**一个变量的时候必须指定**default**

## 打包CSS/Images等资源
webpack本身只针对JS打包的，默认是不支持打包CSS/Images等资源。
webpack提供了一套loader规范，运行自行编写loader来支持各种格式资源的打包。

### 打包CSS资源
1. 安装stye-loader和css-loader依赖
  + syle-loader让JS认识CSS
  + css-loader将CSS装载到JS中
```cmd
yarn add style-loader css-loader -D
```
2. 修改webpack.config.js
```js
const path = require('path')

module.exports = {
    entry: './src/main.js',
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    // 模式有三种，development、production、none
    mode: 'none',

    module: {// 模块设置
        rules: [// 打包规则
            {
                test: /\.css$/, // 匹配css文件
                use: [// 使用loader，顺序不能错
                    'style-loader',
                    'css-loader'
                ]
            }
        ]
    }
}
```
3. 创建style.css文件
在src目录下创建创建css/style.css文件
```css
body {
    background-color: red;
}
```
4. 在main.js中引入css文件
```js
import '../css/style.css'
```
5. 重新打包项目看效果
```cmd
yarn build
```
> 原理是打包时把style.css文件加载到JS中，在浏览器执行的时候会实行这段JS代码，动态添加style标签给body添加样式

### 打包Images资源
1. 安装file-loader
```cmd
yarn add file-loader -D
```
2. 修改webpack.config.js
```js
const path = require('path')

module.exports = {
    entry: './src/main.js',
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    // 模式有三种，development、production、none
    mode: 'none',

    module: {// 模块设置
        rules: [// 打包规则
            {
                test: /\.css$/, // 匹配css文件
                use: [// 使用loader，顺序不能错
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(bmp|png|jpg|gif|svg)$/, // 匹配图片
                use: [// 使用loader
                    'file-loader'
                ]
            }
        ]
    }
}
```
3. 在src目录下创建images目录，放入一张图片
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/200321_cdfde2ae_5449551.png "屏幕截图.png")
4. 修改style.css
```css
body {
    background-color: red;
    background-image: url("../images/background.bmp");
}
```
5. 重新打包
```cmd
yarn build
```
> 打包后刷新页面发现图片未显示，原因时index.html访问图片路径为根目录，而打包后图片放在了/dist目录下，只要把图片移到根目录就可以了。这样很不方便，也不合理，针对这样的问题，可以使用html-webpack-plugin插件解决。

## html-webpack-plugin
作用是解决文件路径的问题。
1. 安装html-webpack-plugin
```cmd
yarn add html-webpack-plugin -D
```
2. 修改webpack.config.js
```js
const path = require('path')

// 引入html-webpack-plugin插件
const HTMLWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    entry: './src/main.js',
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    // 模式有三种，development、production、none
    mode: 'none',

    module: {// 模块设置
        rules: [// 打包规则
            {
                test: /\.css$/, // 匹配css文件
                use: [// 使用loader，顺序不能错
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(bmp|png|jpg|gif|svg)$/, // 匹配图片
                use: [// 使用loader
                    'file-loader'
                ]
            }
        ]
    },
    plugins: [
        // 使用html-webpack-plugin插件
        new HTMLWebpackPlugin({
            // 此插件作用是将 index.html 打包到 bundle.js 所在目录中, 
            // 同时也会在 index.html 中自动的 <script> 引入 bundle.js 
            // 注意：其中的文件名 bundle 取决于上面output.filename中指定的名称
            template: './index.html'
        })
    ]
}
```
3. 修改index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <!-- 模拟一下vue的index.html -->
    <div id="app"></div>
</body>
<!-- 不需要自己引入JS，html-webpack-plugin会自动将JS引入 -->
<!-- <script src="./dist/bundle.js"></script> -->
</html>
```
4. 重新打包
```cmd
yarn build
```
这时看到dist目录中多了index.html，并且自动引入了bundle.js
![输入图片说明](https://images.gitee.com/uploads/images/2019/1225/202437_bf24c4ef_5449551.png "屏幕截图.png")
5. 运行dist目录里的index.html
这时图片就能正常显示了。
> 我们看别人写的Vue的项目，只有一个index.html，并且没有引用任何JS就能跑起来，就是因为有了这个插件。

## 实时重新加载
上面的例子中，每次改动都要重新打包项目才能看到效果，为了解决这个问题，可以采用webpack提供的工具webpack-dev-server，每次修改代码都会自动打包并重新刷新页面。
1. 安装webpack-dev-server
```cmd
yarn add webpack-dev-server -D
```
2. 修改webpack.config.js
```js
const path = require('path')

// 引入html-webpack-plugin插件
const HTMLWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    entry: './src/main.js',
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    // 模式有三种，development、production、none
    mode: 'none',

    module: {// 模块设置
        rules: [// 打包规则
            {
                test: /\.css$/, // 匹配css文件
                use: [// 使用loader，顺序不能错
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(bmp|png|jpg|gif|svg)$/, // 匹配图片
                use: [// 使用loader
                    'file-loader'
                ]
            }
        ]
    },
    plugins: [
        // 使用html-webpack-plugin插件
        new HTMLWebpackPlugin({
            // 此插件作用是将 index.html 打包到 bundle.js 所在目录中, 
            // 同时也会在 index.html 中自动的 <script> 引入 bundle.js 
            // 注意：其中的文件名 bundle 取决于上面output.filename中指定的名称
            template: './index.html'
        })
    ],
    // 实时重新加载，不需要每次修改都手动打包
    devServer: {
        contentBase: './dist'
    }
}
```
3. 执行命令
```cmd
.\node_modules\.bin\webpack-dev-server --open
```
服务就启动了，页面也会自动打开。
4. 修改style.css查看效果
```css
body {
    background-color: red;
    /* background-image: url(../images/background.bmp); */
}
```
这时页面的背景图自动消失，底色变成红色。
5. 简化启动命令
**.\node_modules\.bin\webpack-dev-server --open**
这个命令太长了，我不喜欢，所有使用自定义命令别名搞个简单的。
改一下package.json，在scripts中添加
```json
"dev": ".\\node_modules\\.bin\\webpack-dev-server --open"
```
再执行一下命令
```cmd
yarn dev
```
服务起来了，美滋滋^_^
## Babel 浏览器兼容性
Babel的作用是将ES6语法的代码转换成ES5的语法，让旧版本的浏览器能兼容。
1. 安装Babel
```cmd
yarn add babel-loader @babel/core @babel/preset-env -D
```
2. 修改webpack.config.js
```js
const path = require('path')

// 引入html-webpack-plugin插件
const HTMLWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    entry: './src/main.js',
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    // 模式有三种，development、production、none
    mode: 'none',

    module: {// 模块设置
        rules: [// 打包规则
            {
                test: /\.css$/, // 匹配css文件
                use: [// 使用loader，顺序不能错
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(bmp|png|jpg|gif|svg)$/, // 匹配图片
                use: [// 使用loader
                    'file-loader'
                ]
            },
            {
                test: /\.m?js$/,
                exclude: /(node_modules|bower_components)/, // 排除的目录
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env'] // 内置转换工具
                    }
                }
            }
        ]
    },
    plugins: [
        // 使用html-webpack-plugin插件
        new HTMLWebpackPlugin({
            // 此插件作用是将 index.html 打包到 bundle.js 所在目录中, 
            // 同时也会在 index.html 中自动的 <script> 引入 bundle.js 
            // 注意：其中的文件名 bundle 取决于上面output.filename中指定的名称
            template: './index.html'
        })
    ],
    // 实时重新加载，不需要每次修改都手动打包
    devServer: {
        contentBase: './dist'
    }
}
```
3. 修改main.js
写一些ES6的代码
```js
let a = 1

const arr = [1, 2, 3]

arr.forEach(item => {
    console.log(item)
})
```
4. 打包一下
```cmd
yarn build
```
看到打包后代码编程ES5语法了
![输入图片说明](https://images.gitee.com/uploads/images/2019/1225/210614_8f7d87b4_5449551.png "屏幕截图.png")
## vue-loader打包Vue文件组件
> 在这之前必须保证项目已经安装了vue。

1. 安装vue-loader
```cmd
yarn add vue-loader vue-template-compiler -D
```
2. 修改webpack.config.js
```js
const path = require('path')

// 引入html-webpack-plugin插件
const HTMLWebpackPlugin = require('html-webpack-plugin')

// 1.引入vue-loader插件
const VueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
    entry: './src/main.js',
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    // 模式有三种，development、production、none
    mode: 'none',

    module: {// 模块设置
        rules: [// 打包规则
            {
                test: /\.css$/, // 匹配css文件
                use: [// 使用loader，顺序不能错
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(bmp|png|jpg|gif|svg)$/, // 匹配图片
                use: [
                    'file-loader'
                ]

            },
            {
                test: /\.m?js$/,
                exclude: /(node_modules|bower_components)/, // 排除的目录
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env'] // 内置转换工具
                    }
                }
            },
            // 3.添加vue文件的处理规则
            {
                test: /\.vue$/,
                use: [
                    'vue-loader'
                ]
            }
        ]
    },
    plugins: [
        // 使用html-webpack-plugin插件
        new HTMLWebpackPlugin({
            // 此插件作用是将 index.html 打包到 bundle.js 所在目录中, 
            // 同时也会在 index.html 中自动的 <script> 引入 bundle.js 
            // 注意：其中的文件名 bundle 取决于上面output.filename中指定的名称
            template: './index.html'
        }),
        // 2.使用vue-loader插件
        new VueLoaderPlugin()
    ],
    // 实时重新加载，不需要每次修改都手动打包
    devServer: {
        contentBase: './dist'
    }
}
```
3. 在src目录下添加App.vue
```vue
<template>
  <div>
    <p class="text" v-text="msg"></p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: "我是Vue组件"
    };
  }
};
</script>

<style scoped>
.text {
  color: red;
}
</style>
```
4. 修改main.js
在main.js中引入Vue和App.vue
```js
import Vue from 'vue'
import App from './App.vue'

new Vue({
    el: '#app',
    template: '<app></app>',// Vue实例化时会把template属性的内容加载到el指定的元素中
    components: {
        app: App
    }
})
```
5. 启动服务
```cmd
yarn dev
```
这时发现浏览器报错
![输入图片说明](https://images.gitee.com/uploads/images/2019/1225/213418_71364540_5449551.png "屏幕截图.png")

> 原因分析，打开node_modules目录找到vue目录，打开package.json，发现main属性指向的是dist/vue.runtime.common.js
> Vue分为了两块
> +  编译器 用来编译template模板编程JS渲染函数的代码
> + vue.runtime.common.js 运行时，用来创建Vue实例和渲染处理虚拟DOM对象等代码，没有编译器功能，无法解析template
> dist/vue.js则是Vue的完整版，包含编译器和运行时，如果直接引入dist/vue.js即可解决问题
> 但是这样会由于引入较大的JS库，使得打包的文件太过臃肿
> 最佳解决方法在下面

```js
import Vue from 'vue'
import App from './App.vue'

new Vue({
    el: '#app',
    // 使用rander方法来渲染组件
    // render: function(h){
    //     return h(App)
    // }
    render: h => h(App) // ES6写法
})
```
现在就可以看到效果了。
## 模块热替换（HMR）
模块热替换(hot module replacement 或 HMR)是webpack最有用的功能之一，可以在不刷新整个页面的前提下，局部刷新修改的组件。
1. 修改webpack.config.js
**一共有三步**
```js
const path = require('path')

// 引入html-webpack-plugin插件
const HTMLWebpackPlugin = require('html-webpack-plugin')

// 1.引入vue-loader插件
const VueLoaderPlugin = require('vue-loader/lib/plugin')

// ***模块热部署第一步
const webpack = require('webpack')

module.exports = {
    entry: './src/main.js',
    output: {
        path: path.join(__dirname, './dist/'),
        filename: 'bundle.js'
    },
    // 模式有三种，development、production、none
    mode: 'none',

    module: {// 模块设置
        rules: [// 打包规则
            {
                test: /\.css$/, // 匹配css文件
                use: [// 使用loader，顺序不能错
                    'style-loader',
                    'css-loader'
                ]
            },
            {
                test: /\.(bmp|png|jpg|gif|svg)$/, // 匹配图片
                use: [
                    'file-loader'
                ]

            },
            {
                test: /\.m?js$/,
                exclude: /(node_modules|bower_components)/, // 排除的目录
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env'] // 内置转换工具
                    }
                }
            },
            // 3.添加vue文件的处理规则
            {
                test: /\.vue$/,
                use: [
                    'vue-loader'
                ]
            }
        ]
    },
    plugins: [
        // 使用html-webpack-plugin插件
        new HTMLWebpackPlugin({
            // 此插件作用是将 index.html 打包到 bundle.js 所在目录中, 
            // 同时也会在 index.html 中自动的 <script> 引入 bundle.js 
            // 注意：其中的文件名 bundle 取决于上面output.filename中指定的名称
            template: './index.html'
        }),
        // 2.使用vue-loader插件
        new VueLoaderPlugin(),

        // ***模块热替换第二步
        new webpack.HotModuleRepalcementPlugin()
    ],
    // 实时重新加载，不需要每次修改都手动打包
    devServer: {
        contentBase: './dist',

        // ***模块热替换第三步
        hot: true
    }
}
```
2. 重启服务
```cmd
yarn dev
```
3. 修改App.vue
```vue
<template>
  <div>
    <p class="text" v-text="msg"></p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: "我是Vue组件111" // 随便修改一下查看效果
    };
  }
};
</script>

<style>
.text {
  color: red;
}
</style>
```
现在就可以愉快开发了，哈哈^_^