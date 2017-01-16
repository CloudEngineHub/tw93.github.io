---
layout:     post
title:      Hello Weex
date:       2017-01-06 21:47:29
summary:    本周给部门同学分享了《Hello Weex》这个Topic，现整理成文字版本，分享给关注Weex的同学，主要涉及:Module && Component、Weex Architecture、 Weex Others。

categories: weex
---

![https://pic2.zhimg.com/v2-333789d2dfcce736510e694b696e0f55_b.jpg](https://pic2.zhimg.com/v2-333789d2dfcce736510e694b696e0f55_b.jpg)

本周给部门同学分享了《Hello Weex》这个Topic，现整理成文字版本，分享给关注Weex的同学，主要涉及：

1. Module && Component
2. Weex Architecture
3. Weex Others

*为了确保信息保密，已经将一些其他信息去除，请谅解，*分享开始 👨‍👨‍👧‍👧 🤣。

## **Module && Component**

Module和Component相信有些同学区分不清楚，通过google搜索得到如下两个解释：

> Module:
> An implementation unit of software that provides a coherent set of responsibilities.
>
> Component:
> A component is a reusable building block that can be combined with other components in the same or other computers in a distributed network to form an application.

![](https://pic4.zhimg.com/v2-616b392ce21cef7f1adf173591f9175f_b.png)

大致意思是说，Module是指提供一组连贯的职责的软件的实现单元；Component 是可重复使用的程序构建块，它可以与分布式网络中相同或其他计算机中的其他组件相结合，形成应用程序。

以上的解释更加偏向计算机科学层面。

[@贺师俊](https://www.zhihu.com/people/3ec3b166992a5a90a1083945d2490d38)在[关于前端开发中“模块”和“组件”概念的思考 · Issue \#21 · hax/hax.github.com**](https://link.zhihu.com/?target=https%3A//github.com/hax/hax.github.com/issues/21)这篇文章归纳下大概是如下这样：

Module是指编程语言所提供的代码组织机制，利用此机制可将程序拆解为独立且通用的代码单元。**偏向静态的代码结构，Module更加强调职责**。

Component指功能单元，其意义偏向运行时的结构，并有更复杂的控制，**核心意义在于复用**，相对于Module对依赖性有更高的要求。

那么在Weex中呢，Module和Component具体是什么呢？可以先看下Weex中Module和Component所包含的东西。

**Module是一组能被JS Framework调用的API. 其中的一些能以异步的方式调用JS Framework。**

![](https://pic3.zhimg.com/v2-35766af449daf3d26c86920c59f7f256_b.png)

**Component是指在屏幕内可见，有特定行为，能被配置不同的属性和样式，能响应用户交互。**

![](https://pic4.zhimg.com/v2-b1fc6e86fd782398fd002707ba6e6b77_b.png)

## **Weex Architecture**

官网是这样描述Weex为*"A framework for building Mobile cross-platform UI"，*一款轻量级的移动端跨平台动态性技术解决方案，其实说的通俗点就是Vue-Native 。🤔🤒

相信对Weex有过关注的同学应该都见过下面这个Weex架构图。

![](https://pic1.zhimg.com/v2-0bf033aff1807ae88aebf2f59adfe794_r.png)看图说话是这样的：

1. weex-toolkit中的transform工具将我们所编写的.we文件转换成JS Bundle，然后将JS Bundle部署到Server。
2. Weex SDK中的JS Framework获取到JS Bundle并执行，对其进行实例初始化工作，同时进行数据绑定、模板编译，同时提供 callNative 和 callJS 方法。
3. JS Framework和native通过callNative 和 callJS这两个方法进行通信，共同依赖于一份相同的JSON配置表（此次可以联想到RN的实现原理😉😉）

**Transform所做的工作**其实是将DSL转成类JSON树形结构，并汇总结合成一JavaScript AMD 模块，便于后续过程中JS Framework的处理，可以从如下图看到transform所做的工作：

![](https://pic2.zhimg.com/v2-3163c799a0cbac9978fd114cbbbf7f61_b.png)

以上的左边部分其实就是一个DSL，一种领域特定语言，可以直接使用其对象领域中的概念，集中描述“想要做到什么”（What）的部分，而不必对“如何做到”（How）进行描述。

> A domain-specific language (DSL) is a computer language specialized to a particular application domain.

**JS Framework** 初始化一个实例涉及如下过程，详细见[**vanilla/index.js**](https://link.zhihu.com/?target=https%3A//github.com/alibaba/weex/blob/master/html5/vanilla/index.js)

![](https://pic1.zhimg.com/v2-35311f3ab3158b57ec8d41fa62d54064_b.png)

看到这里的人一定是爱学习的.....

![](https://pic2.zhimg.com/v2-de0a0d9531397ae9e8d27d0c75d1d1d9_b.png)

## **Weex Others**

**1. Why not use scroller ？**

为什么不推荐使用scroller呢？Native的同学肯定知道Android中的ScrollView和Ios中的UIScrollView这两个东西，详细看图：

![](https://pic4.zhimg.com/v2-63eadb0d515aaa8a4725790cb8948043_b.png)

可以把scroller里面的东西想象成一个大的子View，如果列表过长，可想而知首屏的渲染完成和界面可操作需要等到将所有列表加载完成后才能使用，没有内存回收，无疑会对性能和体验造成一定影响。

**2. Why use list ？**

为什么要使用list呢？原因是此component只渲染可见区域，同时可以进行内存复用。

![](https://pic2.zhimg.com/v2-6100f2e022a79fef68ab03373b50d301_b.png)

假如还是不是很好理解，可以看看Ios中UITableView的原理图：

![](https://pic2.zhimg.com/v2-02593ad0bded3db27c0da57333a37b71_r.png)

**UItableView控件使用cell来展示数据。一个cell对应一个row，但是cell和row不完全相同。首先cell是一个view，cell的数量是由在某一刻可以看到的row（行）的数量决定的。当一行数据被往上移动移出屏幕不可见后，cell会被重复利用，接着用来展示新出现在屏幕中的那些行数据。**

list只适合垂直长列表的滚动场景，但是假如需要使用横向滚动就必须使用scroller了。

**3. Weex App**

Weex现在也可以像RN那样生成APP了，详细可以见[**weexteam/weex-hackernews**](https://link.zhihu.com/?target=https%3A//github.com/weexteam/weex-hackernews)，以下是我的运行结果，真正做到了一份代码多处运行。

![](https://pic4.zhimg.com/v2-ebfd33a69b59dac219802e855e527d67_b.png)

**4. Weex-x**

写RN的人可能会嘲笑写Weex的，“看你怎么搞复杂数据管理的东西？👹👹”，RN的开发者可以通过Redux来实现state的良好管理，weex其实也可以的，试试[**Jinjiang/weex-x**](https://link.zhihu.com/?target=https%3A//github.com/Jinjiang/weex-x)吧，可以从如下的example中看到对于的数据流管理。

{% highlight javascript %}
import { Store } in 'weex-x'
const store = new Store({
  state: { firstName: 'Jinjiang', lastName: 'ZHAO' },
  getters: { fullName: state =\> `${state.firstName} ${state.lastName}` },
  mutations: {
    setFirstName (state, name) {
      state.firstName = name
    },
    setLastName (state, name) {
      state.lastName = name.toUpperCase()
    }
  },
  actions: {
    setFirstName: ({ commit }, payload) =\> commit('setFirstName', payload),
    setLastName: ({ commit }, payload) =\> commit('setLastName', payload),
    setFullName({ commit }, payload) {
      const result = payload.split(' ', 2)
      commit('setFirstName', result[0])
      commit('setLastName', result[1])
    }
  }
})
{% endhighlight %}

**5. Weex Vue 2.0****很期待..........**

**完，有描述不对或者不清楚的地方欢迎大家指出。**
