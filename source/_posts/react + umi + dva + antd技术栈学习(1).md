---

title: react + umi + dva + antd技术栈学习

tag: ['react', 'umi', 'dva']

categories: 

- react
- umi
- dva

---
# react + umi + dva + antd技术栈学习

## 概述

**首先目录结构大致为这样**

```
-users
|-index.tsx ------------ 主页面的列表展现
|-index.less ----------- 主页面的样式展现
|-model.ts ------------- 仓库
|-service.ts ----------- 异步接口的逻辑
|-components ----------- 组件文件夹
```

一个完整的React页面就是**View + Service + Model**，分别对应着**index.tsx, service.ts, model.ts**

- react：实现UI界面的一个库，没有考虑到数据的存储和流向问题
- redux：专门处理数据的存储和流向问题
- redux-saga：基于redux，专门处理异步数据
- react-router：基于React之上的强大路由库
- dva：基于redux和redux-saga的数据流方案
  - redux和redux-saga是dva的一个基础
  - 简化开发体验，内置了react-router和fetch
- umi：umi以路由为基础，同时支持配置式路由和约定式路由，保证路由的功能完备，并以此进行功能扩展。然后配以生命周期完善的插件体系
  - umi实际上核心就是react-router
  - 上述dva实现了一部分的react-router，umi是把react-router再次进行重新的整合和约定

## DVA基本流程

一个项目中有**公有数据**和**私有数据**。在redux中共有数据就是Store的概念；在dva中则提出了一个新概念：Model。所以在dva当中，会把Model理解为公共数据，state理解为私有数据。

在dva中，页面和仓库是怎么沟通的？

首先，页面会发起请求，如果是异步数据，需要仓库中转到同步，再返回给页面。也就是页面发送的请求可以是同步也可以是异步，但是回来的时候只有一个口，通过同步返回。

**dva流程图**

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c01db6d8beff492f835bc8af9b0b2d09~tplv-k3u1fbpfcp-watermark.awebp)

Reducer可以理解为水龙头，水的来源可能不同，但是出水口只有一个，也即Effect接收不同的接口数据，然后统一返回给reducer，reducer再统一返回给页面

## 相关api用法

- **useEffect**：在DOM更新完成后执行某些副作用操作，如数据获取，设置订阅以及手动更改React组件中的DOM等

  useEffect是在render之后浏览器已经渲染结束才执行

  1. 没有第二个参数时，在每次render之后都会执行useEffect中的内容
  2. 第二个参数是可选的，类型是一个空数组；第二个参数是非空数组时，useEffect接受第二个参数来控制跳过执行，下次render后如果指定的值没有变化就不会执行
  3. 第二个参数是空数组时，useEffect只在第一次渲染时执行，由于空数组中没有值，始终没有改变，所以后续render不执行，相当于生命周期中的componentDidMount

- **useCallback和useMemo：**都是性能优化的手段，类似于组件中的shouldComponentUpdate，在子组件中使用shouldComponentUpdate，判定该组件的props和state是否变化，从而避免每次父组件render都去重新渲染子组件。二者分别用来缓存函数与对象，但性能优化也会有成本，缓存过多时会占用内存。大多数情况都不应该使用。