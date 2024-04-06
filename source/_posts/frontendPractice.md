---
title: 前端项目开发经验细节记录
date: 2023-07-07
tags: [前端项目, 前端开发]
---

# 项目开发经验细节记录

## 1. 开发

1. 初始化搭建`npm init vue@latest`
2. 了解架构
   - package.json：项目配置文件
   - index.html：入口文件，创建 app 实例并挂载上 DOM
   - src:
     - request：数据传递
     - router：路由
     - store：vuex 状态管理
     - components：组件
     - views：页面
     - assets：资源文件
3. 开发步骤
   - 根据页面结构完成 router 路由，嵌套路由、动态路由
   - 封装 axios，封装 api，拦截器设置 token
   - 利用组件库完成`<template>`和`<script>`部分，完成单个页面的界面元素及布局
   - 前后端联调

## 2. `<style>`

- 初始化：style.css 中加入 reset.css 清除浏览器默认 CSS 样式

- 字体：style.css 中 @font-face 指定一个用于显示文本的自定义字体；字体能从远程服务器或者用户本地安装的字体加载

- icon：使用统一的图标库，建立 iconfont 项目，代码引入项目：<http://www.manongjc.com/detail/29-dcjitrxwpghfocm.html>

- 组件库自定义：使用:deep() 进行穿透，覆盖原组件库样式，利用 F12 寻找目标元素

- 命名规范：css-bem 规范

- 动画：<https://gsap.com>

- 国际化（多语言）
  <https://juejin.cn/post/7082730122809180174>
  - 创建语言包对象：建立多语言的 JSON 文件，原理是 key 一致，value 相对应
  - 组合语言包对象
  - 创建 i18n 实例
  - 挂载到 Vue 上
  - $t 在`<template>`中使用

## 3. 前后端联调

### 3.1 mock.js

基于 node 的 express 框架所搭建的项目：
<http://mockjs.com/>

- 语法规范：
  - 数据模板定义规范：依据规则随机生成数据
  - 数据占位符定义规范：调用官方的一些随机生成函数
- VUE 中使用：
  - 定义路由接口，在接口中返回 mock 模拟的数据
  - 配置 devServer，在 before 层性中引入接口路由函数（优先使用 mock 定义的接口，再去找外网的）
  - 在 vue 页面正常使用接口

### 3.2 联调方式

- 异地联调时，公共服务器

  - 创建一个联调环境，其中包括前端和后端的集成环境

- 在同一局域网下（ping ip 能 ping 通）
  1. 用 postman 等接口工具检测接口运行状态
  2. 前端使用代理跨域以访问接口，具体选择如下：
     - vue-cli：官方插件 vue-cli-plugin-proxy
     - vite：vite.config.js 中配置
     - webpack：vue.config.js 中配置
- 异地联调时，内网穿透

  - 原理：将本地计算机或内部网络暴露到公共网络中的，以用于前后端异地联调，以便远程开发人员可以访问本地环境的服务
  - 工具：https://frp.starryfrp.com/

### 3.3 前端通过 api 向后端传参方式

- api 参数类型概念：

  - Query Parameters（查询参数）:用于筛选、排序和限制 API 的响应。查询参数通常不影响 API 的路由或路径，而是用于传递额外的信息，例如搜索关键字、分页、排序选项等。示例：/api/products?category=electronics&page=2&sort=price

  - Request Body Parameters（请求体参数）:这些参数通常作为 API 请求的请求体中的 JSON 数据或表单数据发送。它们用于传递大量数据，通常用于创建、更新或提交资源。通常用于 POST、PUT 或 PATCH 请求

  - Route Parameters（路由参数）:路由参数通常是 URL 中的一部分，用于定义 API 的路由或路径。它们通常出现在 URL 的特定位置，通常由占位符表示，例如/api/products/:id。这种类型的参数通常用于标识唯一的资源或资源集合，例如获取特定 ID 的产品

  - Header Parameters（头参数）:参数通常包含在 HTTP 请求的头部中。它们用于传递元数据或授权信息，例如身份验证令牌、内容类型、接受语言等

  - Cookie Parameters（Cookie 参数）:Cookie 参数是通过浏览器的 Cookie 来传递的数据，通常用于跟踪用户会话或保存持久性信息

  - Form Data Parameters（表单数据参数）:这些参数通常用于 HTML 表单提交。当用户填写表单并提交时，表单数据参数将以键值对的形式传递到 API

- 区别与应用场景：
  - query
    - get 请求只能传 query 参数，query 参数都是拼接在请求地址上的
    - get 请求在 url 中传送的参数是有长度限制的，而 post 没有限制
    - get 比 post 更不安全，因为参数直接暴露在 url 上，所以不能用来传递敏感信息
  - body
    - post 可以传 body 和 query 两种形式的参数

### 3.4 token or cookie

- token 的传输方式：一般使用请求头 Authorization 字段携带 token，因为前后端分离 set-cookie 不会生效；使用 token 兼容性更好，做 app 也更方便（app 端用不了 cookie）而 token 是任何端都通用的

## 4. 数据可视化

工具：antg，D3，eCharts

## 其他

1. 命令行`mddir`以导出文件目录结构 -> 放置在 README.md 中
