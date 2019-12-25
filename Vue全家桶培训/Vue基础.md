# Vue基础
Vue是由前谷歌工程师，华裔尤雨溪所编写的一个渐进式前端开发框架，轻量却功能强大，代码简洁，开发效率极高，目前非常火的一个前端框架。
官网地址：https://cn.vuejs.org/

## 什么是渐进式
就是可把Vue当作一个类似Jquery那样的JS库来使用，也可以使用Vue全家桶来开发大型项目。
## 与其他前端框架对比
+ Angular
开发于2009年，后来谷歌收购了，核心技术是**模板**和**数据绑定**，现在体验越来越差，开始走下坡路。
+ React
Facebook的内部项目，2013年开源，当时是由于对市场上的前端框架不满意所以自己开发了一个，很任性。核心技术是**组件化**和**虚拟DOM**。
+ Vue
吸收上述框架的技术优点，站在巨人的肩膀上。完善的中文文档，学习成本低。
+ 使用情况
BAT基本的项目：React > Angular > Vue
中小型企业：用Vue多一些
> Vue不支持IE8
## 创建Hello Would项目
1. 在vscode创建工作空间
2. 创建项目目录
3. 使用Yarn初始化项目
4. 安装Vue
```cmd
yarn add vue
```
5. 编写index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Hello Would</title>
</head>
<body>
    <div id="app">
        <input type="text" v-model="msg"/>
        {{msg}}
    </div>
</body>
<script src="./node_modules/vue/dist/vue.js"></script>
<script>
    new Vue({
        el: '#app',// el不能为html和body标签
        data: {
            msg: ''
        }
    })
</script>
</html>
```
## 什么是MVVM
![输入图片说明](https://images.gitee.com/uploads/images/2019/1223/011650_cfadb4f1_5449551.png "屏幕截图.png")

## 安装Vue Devtools
Vue Devtools是专门用于调试Vue应用的chrome插件，可以直接翻墙从扩展商店下载，或者问我要。
安装成功之后扩展栏会出现**Vue**的图标。
![输入图片说明](https://images.gitee.com/uploads/images/2019/1223/012924_50d5294f_5449551.png "屏幕截图.png")

记得在扩展管理那里打开**允许访问文件网址**这一项，不然自己写的本地页面会不起作用。
![输入图片说明](https://images.gitee.com/uploads/images/2019/1223/013136_54a0b8e7_5449551.png "屏幕截图.png")

F12会多出**Vue**这一项
![输入图片说明](https://images.gitee.com/uploads/images/2019/1223/013432_28dff02a_5449551.png "屏幕截图.png")
## Vue指令
### 模板数据绑定渲染
#### 双大括号{{ }}和v-text
两者都是向页面插入文本，{{  }}存在二次渲染，效率较低，v-text只渲染一次，建议使用v-text
双大括号

```html
<body>
    <div id="app">
        <input type="text" v-model="msg"/>
        <p>{{msg}}</p>
        <p>{{msg}}</p>
        <p>{{msg}}</p>
        <p>{{msg}}</p>
        <p>{{msg}}</p>
        <p>{{msg}}</p>
        <p>{{msg}}</p>
        <p>{{msg}}</p>
    </div>
</body>
<script src="./node_modules/vue/dist/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            msg: '哈哈'
        }
    })
</script>
```
v-text
```html
<body>
    <div id="app">
        <input type="text" v-model="msg"/>
        <p v-text="msg"></p>
        <p v-text="msg"></p>
        <p v-text="msg"></p>
        <p v-text="msg"></p>
        <p v-text="msg"></p>
        <p v-text="msg"></p>
        <p v-text="msg"></p>
        <p v-text="msg"></p>
    </div>
</body>
<script src="./node_modules/vue/dist/vue.js"></script>
<script>
    new Vue({
        el: '#app',
        data: {
            msg: '哈哈'
        }
    })
</script>
```
#### v-once
一次插值，不会根据data的变化而变化
```html
<p v-once v-text="msg"></p>
```
#### v-html
插入html代码
```html
<p v-once v-html="msg"></p>
<script>
    new Vue({
        el: '#app',
        data: {
            msg: '<div style="color: red">哈哈</div><script>alert("哈哈")<\/script>'
        }
    })
</script>
```
> 为了防止XSS攻击，Vue禁用了script标签
#### v-bind
数据单向绑定到指定属性中。
```html
<input type="text" v-bind:value="msg" />
<p v-text="msg"></p>
<script>
    new Vue({
        el: '#app',
        data: {
            msg: '哈哈'
        }
    })
</script>
```
> 可以简写成 “**:value**”

如果是想动态调整元素样式也可以用来绑定class和style属性
```html
<p :style="{color: fontColor, fontSize: size}">我是文字</p>
<script>
    new Vue({
        el: '#app',
        data: {
            fontColor: 'red',
            size: '20px'
        }
    })
</script>
```
#### v-on

事件绑定，语法是v-on:event="function"
```html
<button v-on:click="clickMe">点我</button>
<script>
    new Vue({
        el: '#app',
        methods: {
            clickMe: function(){
                alert('呵呵')
            }
        },
    })
</script>
```
> 可以简写成“@click”
### 数据操作
#### 计算属性（computed）
通过改变依赖的值触发，返回计算结果。
```html
<div id="app">
    数学：<input type="text" v-model="mathScore">
    英语：<input type="text" v-model="englishScore">
    总分(方法-单向)：<input type="text" v-model="sumScore()">
    总分(计算属性-单向)：<input type="text" v-model="sumScore1">
</div>
<script>
    new Vue({
        el: '#app',
        data: {
            mathScore: 0,
            englishScore: 0
        },
        methods: {
            sumScore: function () {
                // 控制台输入vm.sumScore每次都会被调用
                console.log('我是方法计算')
                return (this.mathScore - 0) + (this.englishScore - 0)
            }
        },
        computed: {
            sumScore1: function () {
                // 控制台输入vm.sumScore1不会被调用，说明计算属性有缓存，效率高
                console.log('我是计算属性')
                return (this.mathScore - 0) + (this.englishScore - 0)
            }
        }
    })
</script>
```
> 这样写的计算属性是单向绑定，如果在文本框里修改sumScore1的值会报错的
#### 计算属性双向绑定
```html
数学：<input type="text" v-model="mathScore">
英语：<input type="text" v-model="englishScore">
总分(方法-单向)：<input type="text" v-model="sumScore()">
总分(计算属性-单向)：<input type="text" v-model="sumScore1">
总分(计算属性-双向向)：<input type="text" v-model="sumScore2">
<script>
    new Vue({
        el: '#app',
        data: {
            mathScore: 0,
            englishScore: 0
        },
        methods: {
            sumScore: function () {
                // 控制台输入vm.sumScore每次都会被调用
                console.log('我是方法计算')
                return (this.mathScore - 0) + (this.englishScore - 0)
            }
        },
        computed: {
            sumScore1: function () {
                // 控制台输入vm.sumScore1不会被调用，说明计算属性有缓存，效率高
                console.log('我是计算属性')
                return (this.mathScore - 0) + (this.englishScore - 0)
            },
            sumScore2: {
                get: function () {
                    return (this.mathScore - 0) + (this.englishScore - 0)
                },
                set: function (newValue) {
                    let avgScore = newValue / 2
                    this.mathScore = avgScore
                    this.englishScore = avgScore
                }
            }
        }
    })
</script>
```
#### 监听器（watch）
监听到指定属性变化时会调用方法。
```html
数学：<input type="text" v-model="mathScore">
英语：<input type="text" v-model="englishScore">
总分(方法-单向)：<input type="text" v-model="sumScore()">
总分(计算属性-单向)：<input type="text" v-model="sumScore1">
总分(计算属性-双向向)：<input type="text" v-model="sumScore2">
总分(监听器)：<input type="text" v-model="sumScore3">
<script>
    const vue = new Vue({
        el: '#app',
        data: {
            mathScore: 0,
            englishScore: 0,
            sumScore3: 0
        },
        methods: {
            sumScore: function () {
                // 控制台输入vm.sumScore每次都会被调用
                console.log('我是方法计算')
                return (this.mathScore - 0) + (this.englishScore - 0)
            }
        },
        computed: {
            sumScore1: function () {
                // 控制台输入vm.sumScore1不会被调用，说明计算属性有缓存，效率高
                console.log('我是计算属性')
                return (this.mathScore - 0) + (this.englishScore - 0)
            },
            sumScore2: {
                get: function () {
                    return (this.mathScore - 0) + (this.englishScore - 0)
                },
                set: function (newValue) {
                    let avgScore = newValue / 2
                    this.mathScore = avgScore
                    this.englishScore = avgScore
                }
            }
        },
        // 定义监听器方法1
        watch: {
            // 监听数学分数变化
            mathScore: function (newValue) {
                this.sumScore3 = (newValue - 0) + (this.englishScore - 0)
            }
        }
    })
    // 定义监听器方法2
    vue.$watch('englishScore', function (newValue) {
        this.sumScore3 = (this.mathScore - 0) + (newValue - 0)
    });
</script>
```
### 条件渲染
#### v-if
直接从浏览器中销毁元素
```html
<button @click="show = !show">点我</button>
<p v-if="show">我是文字</p>
<script>
    new Vue({
        el: '#app',
        data: {
            show: false
        }
    })
</script>
```
#### v-show
通过css控制元素显示隐藏
```html
<button @click="show = !show">点我</button>
<p v-show="show">我是文字</p>
<script>
    new Vue({
        el: '#app',
        data: {
            show: false
        }
    })
</script>
```
> 频繁切换显示隐藏时用v-show，反之用v-if
### 循环渲染（v-for）
```html
<p v-for="(item, index) in list" :key="index" v-text="item"></p>
<script>
    new Vue({
        el: '#app',
        data: {
            list: ['张三', '李四', '王五']
        }
    })
</script>
```
### 数据双向绑定（v-model）
前面讲过不讲了哦^_^
### 自定义指令
#### 全局指令
```js
// 指令名不要带 v- 
Vue.directive('指令名', { 
    // el 代表使用了此指令的那个 DOM 元素 
    // binding 可获取使用了此指令的绑定值 等 
    inserted: function (el, binding) {
        // 逻辑代码 
    }
})
```
#### 局部指令
```js
directives : {
    '指令名' : {// 指令名不要带 v-
        inserted (el, binding) { 
            // 逻辑代码 
        } 
    } 
}
```
#### 示例
1. 输出文本全部转成大写，字体红色
2. 页面加载时让指定元素获得焦点
```html
<div id="app">
    <p v-style="{color: 'red', text: msg}"></p>
    <input type="text" v-focus>
</div>
<script>
    Vue.directive('focus', {
        inserted(el, bind) {
            el.focus()
        }
    })

    new Vue({
        el: '#app',
        data: {
            msg: 'this is words'
        },
        directives: {
            'style': {
                inserted(el, bind) {
                    el.innerHTML = bind.value.text.toUpperCase()
                    el.style = `color: ${bind.value.color}`
                }
            }
        }
    })
</script>
```
## 过渡和动画
### 什么是过度和动画
在实际场景当中通常会使用过渡和动画来优化元素显示和隐藏的体验，在css中操作trasition来控制过渡，操作animation来控制动画。
### 过滤示例
```html
<style>
    .test-enter-active,.test-leave-active{
        transition: opacity 2s;
    }
    .test-enter,.test-leave-to{
        opacity: 0;
    }
</style>
<button @click="show = !show">点我</button>
<transition name="test">
    <p v-show="show" v-text="msg"></p>
</transition>
<script>
    new Vue({
        el: '#app',
        data: {
            show: false,
            msg: '看不到我'
        }
    })
</script>
```
> 解释一下

![输入图片说明](https://images.gitee.com/uploads/images/2019/1223/104534_2b89033b_5449551.png "屏幕截图.png")
+ v-enter类指元素显示之前的样式
+ v-leave-to是指元素隐藏之后的样式
+ v-leave是指元素隐藏之前的样式
+ v-enter-to是指元素显示之后的样式
+ v-enter-active是指元素显示过程中的样式
+ v-leave-active是指元素消失过程中的样式
> 如果transiation标签上添加了name属性，上述样式会被重命名为<name>-enter-to等

还可以这么玩
```html
<style>
    .test-enter-active,.test-leave-active{
        transition: all 2s;
    }
    .test-enter,.test-leave-to{
        opacity: 0;
        transform: translateX(10px);
    }
</style>
```
### 动画示例
```html
<style>
    .test-enter-active {
        animation: test-an 2s;
    }

    .test-leave-active {
        /*reverse是反向的意思*/
        animation: test-an 2s reverse;
    }

    @keyframes test-an {
        0% {
            /*开始状态，缩小为0*/
            transform: scale(0);
        }

        50% {
            /*1秒时放大1.5倍*/
            transform: scale(1.5);
        }

        100% {
            /*2秒时原始大小*/
            transform: scale(1);
        }
    }
</style>

<button @click="show = !show">点我</button>
<transition name="test">
    <div v-show="show" v-text="msg"></div>
</transition>

<script>
    new Vue({
        el: '#app',
        data: {
            show: false,
            msg: '看不到我看不到我看不到我看不到我看不到我看不到我看不到我看不到我看不到我看不到我看不到我看不到我看不到我'
        }
    })
</script>
```