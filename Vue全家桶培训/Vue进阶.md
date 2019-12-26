# Vue进阶
## 路由
### 安装路由组件
```cmd
yarn add vue-router
```
### 引入页面中
```html
<script src="./node_modules/vue/dist/vue.js"></script>
<script src="./node_modules//vue-router/dist/vue-router.js"></script>
```
> 一定要在vue.js之后引入

### 编写路由代码
**js**
```js
// 定义各路由指向的组件
const User = {
    template: '<div>User {{$route.params.id}}</div>'
}

const Dept = {
    template: '<div>Dept</div>'
}

// 实例化路由
const router = new VueRouter({
    // mode: 'history', // 开启历史路由
    routes: [
        // 动态路径参数 以冒号开头
        { path: '/user/:id', component: User },
        { path: '/dept', component: Dept }
    ]
})

new Vue({
    el: '#app',
    router // router: router
})
```
**html**

```html
<router-link to="/user/10">用户</router-link>
<router-link to="/dept">部门</router-link>
<router-view></router-view>
```
## 关于子路由
```html
<script>
    const User = {
        template: '<div>User {{$route.params.id}}</div>'
    }

    const Dept = {
        template: '<div><div>Dept</div><router-link to="/dept/post">岗位</router-link><router-view></router-view></div>'
    }

    const Post = {
        template: '<div>Post</div>'
    }

    const router = new VueRouter({
        // mode: 'history', // 开启历史路由
        routes: [
            // 动态路径参数 以冒号开头
            { path: '/user/:id', component: User },
            {
                path: '/dept', component: Dept,
                children: [
                    // 这里path前面不要加斜杠
                    { path: 'post', component: Post }
                ]
            }
        ]
    })

    new Vue({
        el: '#app',
        router
    })
</script>
```
## 过滤器
### 什么是过滤器
过滤器会对将要显示的文本进行格式化，然后再进行显示，过滤器并没有覆盖原数据，而是产生了新的数据。
### 使用方式
+ 全局过滤器定义
```js
Vue.filter(过滤器名称, function(value1,[value2,...]){
    // 数据处理逻辑代码
})
```
+ 局部过滤器定义
```js
filters: {
    过滤器名称: function(value1,[value2,...]){
        // 数据处理逻辑代码
    }
}
```
> 过滤器可以用在v-bind和双大括号{{ }}两个地方

### 过滤器示例
```html
<p>{{msg | contentFilter}}</p>
<input type="text" :value="a | sumFilter(b)">

<script>
    Vue.filter('contentFilter', (value) => {
        if (!value) {
            return ''
        }
        return value.toUpperCase().replace('搞笑', '***').replace('SB', '***')
    })

    new Vue({
        el: '#app',
        data: {
            msg: '你那么搞笑，是sb吗？',
            a: 2,
            b: 4,
            sum: 0
        },
        filters: {
            sumFilter(a, b) {
                return a + b
            }
        }
    })
</script>
```
## 自定义插件
### 插件干嘛的
+ 插件是给Vue提供全局功能的，比如全局指令、过滤器等
+ 插件需要实现install方法
+ 使用Vue.use()方法来加载插件，必须在new Vue()之前完成
### 自定义插件示例
+ 定义插件
```js
(() => {
    const MyPlugin = {}
    // 实现install方法
    MyPlugin.install = function (Vue, options) {
        // 全局方法
        Vue.eat = function (what) {
            alert(`Vue在吃${what}`)
        }

        // 添加实例方法
        Vue.prototype.$play = function (what) {
            alert(`Vue在玩${what}`)
        }

        // 全局指令
        Vue.directive('sleep', {
            inserted(el, bind) {
                el.innerHTML = 'Vue在睡觉'
            }
        })

        // 全局过滤器
        Vue.filter('ddd', (value) => {
            return `Vue在打豆豆，不想${value}`
        })
    }

    window.MyPlugin = MyPlugin
})()
```
+ 使用插件
```html
<p v-sleep></p>
<p>{{mom | ddd}}</p>
<button @click="toEat">吃薯片</button>
<button @click="toPlay">玩猫</button>

<script src="./node_modules/vue/dist/vue.js"></script>
<script src="./plugins/MyPlugin.js"></script>

<script>
    // 加载插件
    Vue.use(MyPlugin)

    new Vue({
        el: '#app',
        data: {
            mom: '做作业'
        },
        methods: {
            toEat() {
                Vue.eat('薯片')
            },
            toPlay() {
                this.$play('猫');
            }
        },
    })
</script>
```
## 组件化编程
### 什么是组件化
![输入图片说明](https://images.gitee.com/uploads/images/2019/1224/114638_e71540c7_5449551.png "屏幕截图.png")
一个大型应用有多个独立的，高复用的小组件组成，即插即用，提高应用的开发效率，降低维护成本。**对于Vue而言，每一个组件都具有Vue示例的全部特性。**
### 组件定义
+ 全局组件定义
```js
Vue.component('组件名', {
    template: '定义组件模板',
    data: function () { //data 选项在组件中必须是一个函数 
        return {}
    }//其他选项：methods....
})

new vue({
    el: '#app'
})
```
+ 局部组件定义
```js
const ComponentA = {
    template: '<div>这里是组件A</div>',
    data() {
        return {

        }
    }
}

new Vue({
    el: '#app',
    components: {
        组件名: ComponentA,
    }

})
```
> + 组件的命名可以是驼峰式，也可以是下划线分隔
> + HTML中引用组件的DOM则只能用下划线分隔
> + 官方强烈推荐组件名称全小写用下划线分隔
> + 组件的data属性必须返回一个**function**

### 组件使用示例
```html
<component-a></component-a>
<component-b></component-b>

<script>
    Vue.component('component-a',{
        template: '<div>这里是组件A</div>',
        data() {
            return {

            }
        }
    })

    const ComponentB = {
        template: '<div>这里是组件{{name}}</div>',
        data() {
            return {
                name: 'B'
            }
        },
    }

    new Vue({
        el: '#app',
        components: {
            'component-b': ComponentB
        }

    })
</script>
```
### 父子组件之间信息传递
#### 父传子（props）
1. 父组件调用子组件时传入参数
```html
<component-a :id="1" name="张三"></component-a>
```
2. 子组件通过props属性接收
```js
const ComponentB = {
    template: '<div>姓名是{{name}} id是{{id}}</div>',
    //props: ['id', 'name'],
    // props: {
    //     id: Number,
    //     name: String
    // },
    props: {
        id: {
            type: Number,
            required: true,
            defalut: 1
        },
        name: {
            type: String,
            required: false
        }
    },
    data() {
        return {
        }
    },
}
```
> 传递参数的type类型有：**Number、String、Boolean、Array、Object、Function**

#### 父传子（slot）
1. 子组件中定义插槽（slot）
```js
const ComponentB = {
    template: '<div><slot name="a"></slot><slot name="b"></slot></div>',
    data() {
        return {
        }
    }
}
```
2. 父组件通过插槽给子组件传递数据和标签
```html
<!-- 向指定名称（name）的插槽插入内容-->
<component-b><span slot="a">姓名是{{name}}</span><span slot="b">id是{{id}}</span></component-b>

<script>
    new Vue({
        el: '#app',
        data: {
            id: 1,
            name: '张三'
        },
        components: {
            'component-b': ComponentB
        }
    })
</script>
```
#### 子传父（$emit）
1. 父组件传入自定义事件
```html
<component-b @myevent="getSubData"></component-b>

<script>
    new Vue({
        el: '#app',
        components: {
            'component-b': ComponentB
        },
        methods: {
            getSubData(value){
                alert(value)
            }
        },

    })
</script>
```
2. 子组件通过$emit触发父组件传入的事件
```js
const ComponentB = {
    template: '<div><button @click="send">传</button><div>子组件{{name}}</div></div>',
    data() {
        return {
            name: '张三'
        }
    },
    methods: {
        send(){
            // 通过调用vue实例的$emit方法触发事件
            // 第一个参数是事件名称
            // 第二个参数书传入事件方法的参数，可以传多个
            this.$emit('myevent', `我叫${this.name}`)
        }
    }
}
```
### 非父子组件信息传递
有两种方式：
+ 通过PubSubJS消息订阅组件完成
+ 通过Vuex状态保持组件完成（后面讲）
#### PubSubJS方式
1. 安装PubSubJS库
```cmd
yarn add pubsub-js
```
2. 组件A添加消息订阅方法
```html
<script src="./node_modules/pubsubjs/pubsub.js"></script>
```
```js
const ComponentA = {
    template: '<div>这里是组件A</div>',
    data() {
        return {
                
        }
    },
    created() {// created生命周期会在组件被创建后触发
        this.getData()
    },
    methods: {
        getData() {
            // 建立消息订阅
            PubSub.subscribe('myevent', (event, data) => {
                alert(data)
            })
        }
    }

}
```
3. 组件B发送消息
```js
const ComponentB = {
    template: '<div><button @click="send">传</button><div>这里是组件B</div></div>',
    data() {
        return {
            name: '张三'
        }
    },
    methods: {
        send() {
            // 发送数据
            PubSub.publish('myevent', `我是${this.name}`)
        }
    }
}
```
## 生命周期
### 生命周期钩子函数
1. 初始化阶段
    + beforeCreate 实例创建前，模板和数据未获取到
    + created 实例创建之后，最早获取到data数据，模板未获取到
    + beforeMount 数据挂载前，模板以获取到，但是未挂载数据到模板中
    + mounted 数据挂载后，数据已挂载到模板中
2. 更新阶段
    + beforeUpdate data数据已变化，但未更新到模板中
    + updated 将数据渲染到模板后
3. 销毁阶段
    + beforeDestroy 实例销毁前
    + destroy 实例销毁后
![输入图片说明](https://images.gitee.com/uploads/images/2019/1224/135114_bfc3f819_5449551.png "屏幕截图.png")
### 示例代码
```html
<script>
    new Vue({
        //el: '#app',
        beforeCreate() {
            console.log(`beforeCreate:${this.$el},${this.$data}`)
        },
        created() {
            console.log(`created:${this.$el},${this.$data}`)
        },
        beforeMount() {
            console.log(`beforeMount:${this.$el},${this.$data}`)
        },
        mounted() {
            console.log(`mounted:${this.$el},${this.$data}`)
        },
        beforeUpdate() {
            console.log(`beforeUpdate:${this.$el.innerHTML},${this.$data}`)
        },
        updated() {
            console.log(`updated:${this.$el.innerHTML},${this.$data}`)
        },
        beforeDestroy() {
            console.log('beforeDestroy')
        },
        destroyed() {
            console.log('destroyed')
        },
    }).$mount('#app')// 如果没有给定el属性可以这样加载DOM
</script>
```
## 在Vue中使用Ajax
在Vue1.x版本中推荐使用的是vue-resource

官网文档是这个：https://github.com/pagekit/vue-resource/blob/develop/docs/http.md
我们先用的是Vue2.x版本，官方推荐用的是Axios

###  Axios基础
1. 安装Axios库
```cmd
yarn add axios
```
2. 页面引入axios
```html
<script src="./node_modules/axios/dist/axios.js"></script>
```
3. 使用axios发请求

  官网文档是这个：https://github.com/axios/axios/blob/master/README.md
+ 创建假数据/public/db.json做测试
```json
{
    "code": 2000,
    "flag": true,
    "message": "获取成功",
    "data": {
        "id": 10,
        "username": "zhangsan",
        "name": "张三",
        "age": "25",
        "mobile": "13800138000",
        "salary": "5500",
        "entryDate": 1564577990837
    }
}
```
+ 发请求代码
```js
axios.get('public/db.json').then(response => {
    console.log(response)
}).catch(error => {// 当resoponse返回的status为400以上时调用catch
    console.log(error)
})
```
> 如果用文件地址访问的话会报跨域错误，所以需要以http形式访问，vscode需要安装Live Server插件
> 注意返回的response对象，里面的data才是我们返回的数据

<img src="https://images.gitee.com/uploads/images/2019/1225/111302_61fa7820_5449551.png" alt="输入图片说明" title="屏幕截图.png" style="zoom:150%;" />
在vscode界面右下角会多出**GO LIVE**图标，点击即可
![输入图片说明](https://images.gitee.com/uploads/images/2019/1225/111627_acde6576_5449551.png "屏幕截图.png")
### Vue+Axios
```html
<p v-text="`ID：${user.id}`"></p>
<p v-text="`用户名：${user.username}`"></p>
<p v-text="`姓名：${user.name}`"></p>
<p v-text="`年龄：${user.age}`"></p>
<p v-text="`手机：${user.mobile}`"></p>
<p v-text="`年薪：${user.salary}`"></p>
<p v-text="`入职日期：${user.entryDate}`"></p>

<script>
    new Vue({
        el: '#app',
        data: {
            user: {
                id: 0,
                username: '',
                name: '',
                age: 0,
                mobile: '',
                salary: 0,
                entryDate: 0
            }
        },
        created() {// created生命周期是最早能得到data的地方，所有一般在这里渲染初始化数据
            this.getData()
        },
        methods: {
            getData() {
                axios.get('public/db.json').then(response => {
                    let resp = response.data
                    if (resp.code === 2000) {
                        this.user = resp.data
                    }
                })
            }
        },
    })
</script>
```
> created生命周期是最早能得到data的地方，所有一般在这里渲染初始化数据

## 了解前端项目构建打包
[webpack介绍](webpack介绍.md)

## Vue CLI脚手架
### 什么是Vue CLI
Vue CLI是Vue官方提供的快速项目构建枸橘，集成了webpack，内置了大量常用工具，使项目构建更加标准化，是使用Vue全家桶开发项目的必备利器。
### 安装Vue CLI
可全局安装和局部安装。
```cmd
yarn global add @vue/cli -D
```
### 使用Vue构建项目
```cmd
vue create
```
> 如果没有配置yarn全局安装目录的环境变量，这里会报找不到命令的错误

Vue会让你为项目做一些初始化选项
1. 选择项目需要的组件库依赖
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/104953_2cfd5c7e_5449551.png "屏幕截图.png")
选择第二项我们会看到很多选项
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/105125_1bf08aec_5449551.png "屏幕截图.png")
    + Babel：将项目中ES6规范代码转换成ES5规范
    + TypeScript：简称TS，微软开发脚本语言，与JavaScript相比，TypeScript  进步的地方包括：加入注释，让编译器理解所支持的对象和函数，编译器会移除注释，不会增加开销；增加一个完整的类结构，使之更像是传统的面向对象语言。选择这一项会为项目添加对TS的支持。
    + Progressive Web App (PWA) Support：[问简书](https://www.jianshu.com/p/098af61bbe04)，选择该项会为项目提供PWA支持。
    + Router：使用路由组件，选择该项会添加Vue Router依赖
    + Vuex：Vue官方提供的状态存储工具，选择该项会添加Vuex依赖
    + CSS Pre-processors：添加CSS预处理语言支持，比如sess、less和stylus
    + Linter / Formatter：添加代码检查工具依赖
    + Unit Testing：添加单元测试组件依赖
    + E2E Testing：添加模拟黑盒测试工具依赖，E2E测试和单元测试的区别是单元测试代码需要根据单元逻辑的变化而变化，验证逻辑的准确性，而E2E是对需求的验证，不管单元逻辑如何改变，只要需求不变，E2E的测试代码是不需要改变的。
> 按空格选择某一项，按回车保持选择设置

2. 如果选择了Router会询问是否选择历史路由
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/113028_373027ef_5449551.png "屏幕截图.png")
3. 如果选择了Linter / Formatter会询问选择哪种代码检查配置，这里推荐选择第4项
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/113210_7daf5153_5449551.png "屏幕截图.png")
然后又会问什么时候进行代码检查，建议选择保存时就进行代码检查
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/113404_cec91c9c_5449551.png "屏幕截图.png")
4. 接下来会询问各个组件的配置文件保存方法，分开保存还是全部保存到package.json，这里推荐选择第1项分开保存
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/113532_f903ca50_5449551.png "屏幕截图.png")
5. 最后会问是否要将刚才的选择想保存成个性化设置，以后创建项目可以直接使用，不必再次选择
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/113813_4a4af71f_5449551.png "屏幕截图.png")
如果选择是，会让你为自己的设置起个名字
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/114050_c9c96b4a_5449551.png "屏幕截图.png")
6. 如果电脑上同时安装了Yarn和NPM，会询问使用哪个工具来管理项目依赖
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/114150_e4fbf4c8_5449551.png "屏幕截图.png")
> 完成以上所有选项后，项目就会自动构建，耐心等待......

项目构建成功后会看到项目目录结构是这样的
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/114438_04c39dde_5449551.png "屏幕截图.png")
+ public目录存放不会被webpack压缩编译的资源
+ assets目录存放静态资源，如图片、CSS等
+ components目录存放Vue小组件
+ router目录存放路由组件
+ store目录存放Vuex的代码
+ views目录存放页面组件，按照Vue的规范框架页面是views，框架内容组件是components
> Vue CLI默认提供了构建命令，查看package.json

```json
"scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  }
```
## 前端项目的部署
+ 一般选择Nginx，有时会选择NodeJS，极少使用Tomcat
+ 这里只介绍Nginx部署方式
+ Nginx下载地址：http://nginx.org/en/download.html
+ 下载后解压就可以用
1. 打包项目
```cmd
yarn build
```
打包后的结构是这样的
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/120118_891aa321_5449551.png "屏幕截图.png")

2. 将dist目录重命名成项目名称，再将整个目录复制到Ngin的html目录下
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/120906_90831c61_5449551.png "屏幕截图.png")
3. 修改Nginx配置文件nginx/conf/nginx.conf
找到下面这一项
```
location / {
    root   html;
    index  index.html index.htm;
}
```
改成这样
```
location / {
    root   html/vue-project-training;
    index  index.html index.htm;
}
```
4. 启动Nginx服务
在Nginx根目录进入命令行，输入命令
```cmd
nginx
```
停止Nginx服务命令
```cmd
nginx -s stop
```
5. 浏览器访问localhost
![输入图片说明](https://images.gitee.com/uploads/images/2019/1226/121718_c5f14010_5449551.png "屏幕截图.png")