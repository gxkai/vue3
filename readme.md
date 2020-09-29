title: 使用 Vue3 开发小程序
subtitle: Vue3的新特性主要有 Composition API、Teleport、Fragments 等。我们是否也可以在小程序开发中使用这些特性呢？
cover: https://img14.360buyimg.com/ling/jfs/t1/114796/40/19245/125315/5f715516E5765ae26/2375d621a3c73e76.png
category: 小程序
tags: 
    - Vue3
    - Taro3
    - Taro
    - 小程序
author:
    nick: lillian
date: 2020-09-28 09:00:00
---

## 前言

9 月 19 日凌晨，Vue3 在经过多个开发版本的迭代后，终于迎来了它的正式版本，"One Piece" 的代号也昭示了其开拓伟大航路的野心。

Vue3的新特性主要有 Composition API、Teleport、Fragments 和 `<script setup />` & `<style vars />` 等。我们是否也可以在小程序开发中使用这些特性呢？在 Taro 的文档里我们找到了[关于 Vue3 的章节](https://taro-docs.jd.com/taro/docs/vue3)，事不宜迟，让我们开始尝试吧。

## Vue3 部分新特性

还没了解过 Vue3 的同学也别急，先了解下Vue3的新特性吧：

### 1.Composition API

Vue2.X 基于 Option API（选项API）构建组件，一般来说组件拥有 data、methods、computed 等选项。这是一种属性相互隔离的模式，好处是各属性内容分离开，对于新手来说比较友好；但对于大型项目来说，为了修改某个功能，可能需要在一个文件中来回翻页。Vue3 增加了 Composition API 方式（组合 API ），基于 reactivity（响应式）的思想进行组件构建，将逻辑封装到函数中，可以实现类似ReactHooks 的逻辑组合和重用。对于大型项目，代码按照具体功能划分，而不是分散在不同的生命周期中，逻辑更加一目了然。

![图片](https://uploader.shimo.im/f/H3f9IZwZpnsMdPhg.png!thumbnail)

### 2.Teleport（传入）

Teleport功能，使得我们可以将全屏的组件（例如Toast）移到Vue APP节点外，而不需要在UI界面上修改其他组件样式，方便实现全屏蒙层、浮层弹窗等效果。

### 3.Fragments（碎片）

Vue2.x版本中，`<template />` 标签下不支持放置多个组件，这个限制在Vue3中不再存在。这点比较好理解，下述组件设计在Vue3中是没有问题的：

```plain
<!-- Layout.vue -->
<template>
  <header>...</header>
  <main v-bind="$attrs">...</main>
  <footer>...</footer>
</template>
```

### 4.实验性质的语法糖 `<script setup>、<style vars>`

a.`<script setup>`:用于在 SFC 中使用 Composition API 的语法糖，改善在单个文件组件中使用Composition API时的体验。

b.`<style vars>`: SFC 中状态驱动的 CSS 变量，它提供了直接的CSS配置和封装，支持将组件状态驱动的CSS变量注入到“单个文件组件”样式中。

除了以上 4 点之外，Vue3 还提供了自定义渲染，详细可以参考文末的推荐阅读文章。

## 如何在 Taro 里使用 Vue3

Vue3 带来的新特性可以让我们更加优雅和高效地进行开发，现在，来开启我们在 Taro 里使用 Vue3 的体验之旅吧。

Taro 已经默认安装 Taro3 ，所以正常安装即可：

```plain
# 使用 npm 安装 CLI
$ npm install -g @tarojs/cli
# OR 使用 yarn 安装 CLI
$ yarn global add @tarojs/cli
# OR 安装了 cnpm，使用 cnpm 安装 CLI
$ cnpm install -g @tarojs/cli
```
安装完成之后，使用`taro --version`查看一下是否安装成功，如果输出版本号说明安装成功。
安装成功后，初始化一个项目

```plain
$ taro init
```
将出现如下图的一些选型
![图片](https://uploader.shimo.im/f/QfwAArJtITPvi3gV.png!thumbnail)

如上图：

1. 请输入项目名称？输入项目名称`vuedemo`
2. 请输入项目介绍？输入项目介绍`a demo project`
3. 请选择框架？当然是`Vue3`啦
4. 余下选项，如上面 3 个选项，根据自己的需要选择就好，不会有什么问题

一般情况下，依照提示选型好以后， Taro会自动安装依赖。如果由于网络问题导致安装不成功，就需要手动在项目路径下进行安装。

```plain
# 使用 yarn 安装依赖
$ yarn
# OR 使用 cnpm 安装依赖
$ cnpm install
# OR 使用 npm 安装依赖
$ npm install
```
安装后目录结构：
![图片](https://uploader.shimo.im/f/biP5hrZFvyv8SJg5.png!thumbnail)

`app`默认代码如下，我们注意到，Taro3 在新建 Vue3 项目时已经修改了入口组件写法。

```plain
import { createApp } from 'vue'
import './app.scss'
const App = createApp({
  onShow (options) {},
  // 入口组件不需要实现 render 方法，即使实现了也会被 taro 所覆盖
})
export default App
```
`page/index`目录下`index`文件示例如下：
```plain
<template>
  <view class="index">
    <text>{{ msg }}</text>
  </view>
</template>
<script>
import { ref } from 'vue'
import './index.scss'
export default {
  setup () {
    const msg = ref('Hello world')
    return {
      msg
    }
  }
}
</script>
```
编译运行微信小程序
```plain
npm run dev:weapp
```
编译后，打开微信开发者工具导入编译后的`dist`目录，首页内容和编译成 H5的表现都如下图：
![图片](https://uploader.shimo.im/f/pzxlgUkWCEgGpvdC.png!thumbnail)

## 验证Taro3对Vue3的支持度

### Composition API

写个比较简单的 todolist 计数组件，假设目前已有 4 项代办事项，点击后将新增一项。这里会使用到Composition API 思路，以及 computed计算属性。

在用户点击时，第二行“当前操作新增”会根据点击次数递增，总计条数会在 4的基础上累加。

```plain
<template>
  <button @tap="increment">
    增加 1
  </button>
  <view>当前todolist事项已有：{{ existCount }}条；</view>
  <view>当前操作已新增：{{ count }} ，共有{{ total }}条。</view>
</template>
<script>
import { ref, computed, onMounted, toRefs, watch } from 'vue'
export default {
  name: 'case1',
  setup(props) {
    // ref响应式变量
    const count = ref(0)
    const existCount = ref(4)
    // computed方法，在count的value发生改变时，会触发计算total
    const total = computed(() => count.value + existCount.value )
    function increment() {
      count.value++
    }
    
    onMounted(() => console.log('component mounted!'))
    
    return {
      // 返回increment方法，existCount、count、total属性，供模板中调用
      increment,
      existCount,
      count,
      total,
    }
  }
}
</script>
```
页面表现如下所示，可见对于 Composition API 的支持的还是不错的。

![todo计数](https://img14.360buyimg.com/imagetools/jfs/t1/122176/18/13461/1349845/5f6c5d14Eaffd062c/f36c7a7279f8aac6.gif)

### Teleport

写个初次登录用户的欢迎弹窗。用户名从`index.vue`传入。首页代码如下：

```plain
<template>
  <view class="index">
    <Toast :user = username />
    <view id="teleportToast"></view>
  </view>
</template>
```
在 Toast.vue 中，我们会写个属性 to 为 teleportToast 的 Teleport 组件，代码如下：
```plain
<template>
  <button @tap="showToast" class="btn">打开弹窗</button>  
  <!-- to和index.html中的view id一致 -->
  <teleport to="#teleportToast">
    <view v-if="toastFlag" class="toast__wrap"  @tap="hideToast">
      <view class="h2">弹窗标题：</view>
      <view class="toast__wrap--msg">欢迎{{ user }}，点击关闭</view>
    </view>
  </teleport>
</template>
<script>
import { ref,toRefs } from 'vue';
export default {
  props:{
    user: {type: String}
  },
  setup(props) {
    // 获取用户名
    const { user } = toRefs(props)
    // Toast：显示flag
    const toastFlag = ref(false)
    let timer
    const showToast = ()=>{
      toastFlag.value = true
    }
    const hideToast = ()=>{
      toastFlag.value = false
    }
    return {
      user,
      toastFlag,
      showToast,
      hideToast,
    }
  }
}
</script>
<style lang="scss">
.toast__wrap{
  position: fixed;
  width:100%;
  height: 100%;
  background: rgba(0,0,0,.8);
  top: 0;
  left: 0;
  z-index: 101;
  color: #fff;
  .h2{
    margin: 20px;
  }
  &--msg{
    text-align: center;
  }
}
</style>
```
在 H5 下是正常显示的，弹窗展示以及关闭功能效果如下；在小程序上却报错了，Taro 团队还需要加把劲：

![Teleport](https://img10.360buyimg.com/imagetools/jfs/t1/111884/14/18645/2307115/5f6c6e91E963e9fcc/0b7230301e9d2e0a.gif)

### Fragments

Fragments 特性已经在上面的例子中得到验证，使用没有问题。

### script setup 语法糖

我们尝试一下`<script setup>`，组件里的代码如下：

```plain
<template>
  <view>
    <view>count:{{ count }},msg:{{ info }}</view>
    <button @tap="incAndChangeInfo">
      增加 1修改msg
    </button>
  </view>
</template>
<script setup=" props ">
  import { ref, toRefs } from 'vue'
  export default{
    props: {
      msg: String,
    },
  }
  export const count = ref(0)
  export const info = ref(props.msg)
  export const incAndChangeInfo = () => {
    count.value++
    info.value = "change hello" + count.value
  }
</script>
```
上述`script`标签里的代码效果等同于下面：
```plain
<script>
import { ref, toRefs } from 'vue'
export default {
  props: {
    msg: String,
    },
  setup(props) {
  const count = ref(0)
  const info = ref(props.msg)
  const incAndChangeInfo = () => {
    count.value++
    info.value = "change hello" + count.value
    }
  return {
    count,
    info,
    incAndChangeInfo,
    }
  }
}
</script>
```
调用它的代码传入 mgs 如下：
```plain
<Setup msg="hello"/>
```
运行后，小程序和 H5 都是支持的，页面整体表现如下：

![script setup 语法糖](https://img10.360buyimg.com/imagetools/jfs/t1/132702/4/11153/1478203/5f705361E4db3e0cc/d32765423246de84.gif)

可以看到，运用新特性进行开发，代码书写更加便捷，逻辑更清晰。

### style vars 语法糖

`<style vars>`，组件里的代码如下：

```plain
<template>
  <view class="text">文字颜色为{{ color }},点击后变为红色</view>
</template>
<script>
import { ref,toRefs } from 'vue'
export default {
  props:{
    color: {type: String}
  },
   data(props) {
    return {
      color: ref(props) //'red'
    }
  },
  setup(props){
    const { color } = toRefs(props)
    return {
      color,
    }
  }
}
</script>
<style vars="{ color }">
.text {
  color: var(--color);
}
</style>
```
调用它的页面里的代码如下：
```plain
<template>
  <view class="index">
    <Styledemo :color = color @tap="changeColor"/>
  </view>
</template>
<script>
import { ref, computed, onMounted, toRefs, watch } from 'vue'
import Styledemo from "@/floors/styledemo"
export default {
  components:{
    Styledemo
  },
  setup () {
    const color = ref('blue')
    const changeColor = ()=>{
      color.value = 'red'
    }
    return {
      changeColor,
      color
    }
  }
}
</script>
```
小程序和 H5 都没有问题，功能效果如下：

![style vars 语法糖](https://img11.360buyimg.com/imagetools/jfs/t1/146777/39/9447/1836911/5f702d5eE52e470f9/dea5082ff993a478.gif)

## 结语

我们将上述几个Demo 整合在一个项目中，放在[Github](https://github.com/lillian98/vue3)上，有兴趣的同学可以看看。在线预览地址在[这里](https://lillian98.github.io/vue3/dist/#/pages/index/index)。

经过验证，Taro3 支持使用最新的Vue3开发多端应用，相比之下H5的支持度更好一些。

值得一提的是，Taro 团队在技术上一直保持进取，在Taro2.0 版本支持了ReactHooks；根据[Taro RFCS](https://github.com/NervJS/taro-rfcs/blob/master/rfcs/0001-vue-3-support.md)，早在`2020-06-03`也已经打算支持Vue3 了。截至目前，Taro 对 Vue3 的支持在小程序端的稍有补足，希望 Taro 团队可以早日补足这个短板。

推荐文章：

* [Vue3和React ](https://zhuanlan.zhihu.com/p/133819602)[H](https://zhuanlan.zhihu.com/p/133819602)[ooks对比](https://zhuanlan.zhihu.com/p/133819602)
* [SWR](https://github.com/vercel/swr)
* [自定义渲染器的应用](http://hcysun.me/vue-design/zh/renderer-advanced.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%B8%B2%E6%9F%93%E5%99%A8%E7%9A%84%E5%BA%94%E7%94%A8)

参考文章：

* [1][Compsition API](https://v3.cn.vuejs.org/guide/composition-api-introduction.html)
* [2][Teleport](https://v3.cn.vuejs.org/guide/teleport.html#%E4%B8%8E-vue-components-%E4%B8%8E-vue-components-%E4%B8%80%E8%B5%B7%E4%BD%BF%E7%94%A8)
* [3]`<script setup>`
* [4]`<style vars>`
* [5][Taro3安装及使用](https://taro-docs.jd.com/taro/docs/GETTING-STARTED)
## 

