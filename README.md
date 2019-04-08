# vue-router 解读

学习并实现一版简易的`vue-router`。

## 抛出问题

如何在没有`vue-router`等路由组件的情况下开发SPA？

- 保证浏览器URL改变无刷新
- 页面内容可以根据URL路径动态渲染
- 提供路由相关操作API

## 什么是路由

简单来说，路由就是用来和后端服务器进行交互的一种方式，通过不同的路径，请求不同的资源，请求不同的页面是路由的其中一种功能。

## 两种模式

### hash模式

类似于`htttp://blog.careteen.wang/#/login`，`#`后面为`hash`部分，`hash`值变化，不会刷新页面，也就是浏览器不会向服务端发送请求，但会触发`hashchange`事件，通过监听这个事件，可以根据不同`hash`渲染不同视图。

### history模式

由H5的API`pushState`和`replaceState`去改变`url`但不会刷新页面，会触发`popState`事件，和`hash`模式原理一样，只是url更加美观，少了`#`，但是当用户刷新页面时，浏览器会向服务端发送请求，所以需要后端配置所有页面都重定向到根页面。

## 期望提供功能

- 如何挂载到`Vue`？
- 路由嵌套？
- 路由参数、查询、通配符？
- 重定向和别名
- 区分`hash`和`history`模式？
- 实现`router-view`和`router-link`组件？
- 为所有组件提供`$route`即当前路由信息和`$router`即操作路由的方法。
- 导航守卫
  - 全局
  - 路由
  - 组件
  - 完整的导航解析流程
    - 1. 导航被触发
    - 2. 在失活的组件里调用离开守卫`beforeRouteLeave`
    - 3. 调用全局的`beforeEach`守卫
    - 4. 在复用组件中调用`beforeRouteUpdate`守卫
    - 5. 在路由配置中调用`beforeEnter`守卫
    - 6. 解析异步路由组件
    - 7. 在被激活的组件里调用`beforeRouteEnter`守卫
    - 8. 调用全局的`beforeResolve`守卫
    - 9. 导航被确认
    - 10. 调用全局的`afterEach`守卫
    - 11. 触发`DOM`更新
    - 12. 用创建好的实例调用`beforeRouteEnter`守卫中传给`next`的回调函数
  - 实现路由元信息
  - 实现路由懒加载

## 示例

## 源码解析

- 路由注册，挂载到`Vue`实例上
- `VueRouter`对象
  - 初始化
  - 提供静态方法`install`
- `Matcher`
  - 通过`createMatcher`提供`pathList/pathMap/nameMap`
    - `pathMap`存放路径和组件相关信息
  - `match`:根据新老路径匹配为`transitionTo`做铺垫
- `transitionTo`路径切换
  - 路由导航
    - 同步执行异步队列 （实现思路和`koa`中间件原理一样）
  - `url`变化
  - 组件
    - `router-view`
      - 根据路由定义的层级关系确定`router-view`渲染的组件
      - 用`depth`确定嵌套的深度
    - `router-link`
- 总结
  - 路由切换过程
    - 先执行一系列导航守卫钩子函数
    - 更改url
    - 渲染对应的组件