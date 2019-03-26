# vue-router 解读

学习并实现一版简易的`vue-router`。

## 抛出问题

如何在没有`vue-router`等路由组件的情况下开发SPA？

- 保证浏览器URL改变无刷新
- 页面内容可以根据URL路径动态渲染
- 提供路由相关操作API

## 思路

- 如何挂载
- 区分`hash`和`history`模式
- 实现`router-view`和`router-link`组件
- demo