# 小程序原理

## 小程序特点

- 类 WEB 不是 HTML5。
- 即用即走，随手可得。
- 拥有离线能力。
- 基于微信跨平台。
- 媲美原生操作体验。

::: warning 小程序为什么快？

Native 预先加载一个 WebView 当打开指定页面时，无需加载额外资源直接渲染。

返回显示历史 View 退出小程序，View 状态不销毁。
:::

## 小程序架构

小程序分为 3 个部分，视图层，逻辑层，和 系统层。

视图层包括 WXML 和 WXSS。
逻辑层包括我们写的 js。

视图层和逻辑层采用 [JsBridge](/blog/js-jsBridge.html) 进行通信。

![小程序架构图](/blog/wx-jiagou.png)

小程序运行时：先从 cdn 上获取小程序需要的包。

![小程序架构图2](/blog/wx-jiagou2.png)

## 视图层

视图层运行环境：

- 在 WebView 中运行。

### WXML

WXML(WeiXin Markup Language)

支持数据绑定 支持逻辑算术、运算 支持模板、引用 支持添加事件(bindtap)。

WXML 运行流程：
![WXML运行流程](/blog/wx-wxml.png)

### WXSS

WXSS(WeiXin Style Sheets)

- 支持大部分 CSS 特性。
- 添加尺寸单位 rpx，可根据屏幕宽度自适应。
- 不支持多层选择器-避免被组件内结构破坏。（因为生成到 WebView 中的 dom 树和实际的 WXML 会有差异。）

WXSS 运行流程：
![WXSS运行流程](/blog/wx-wxss.png)

### 小程序组件

小程序的组件基于 Web Component 标准 使用 Polymer 框架实现 Web Component。

## 逻辑层

逻辑层将数据进行处理后发送给视图层，同时接受视图层的事件反馈。

1、App( ) 小程序的入口。

2、Page( ) 页面的入口。

3、提供丰富的 API，如微信用户数据，扫一扫，支付等微信特有能力。

4、每个页面有独立的作用域，并提供模块化能力。

5、数据绑定、事件分发、生命周期管理、路由管理。

6、创建新的 WebView，加快下次渲染。

![逻辑层](/blog/wx-appService.png)

如图所示：

- 一个小程序包含 1 个逻辑层和多个视图层。
- 每打开一个页面，就会创建一个新的 WebView，创建 WebView 是比较消耗资源和耗时的，所以微信限制最多只能嵌套 10 层。

逻辑层运行环境：

- IOS - JSCore。
- Android - X5 JS 解析器。
- DevTool - nwjs Chrome 内核。

## 小程序开发建议

::: tip 小程序开发建议
1、提前新建 WebView，准备新页面渲染。

2、View 层和逻辑层分离，通过数据驱动，不直接操作 DOM。

3、使用 Virtual DOM，进行局部更新。

4、全部使用 https，确保传输中安全。

5、前端组件化开发。

6、加入 rpx 单位，隔离设备尺寸，方便开发。
:::

## 小程序存在的问题

::: tip 小程序存在的问题
1、小程序仍然使用 WebView 渲染，并非原生渲染。

2、需要独立开发，不能在非微信环境运行。

3、开发者不可以扩展新组件。

4、服务端接口返回的头无法执行，比如:Set-Cookie。

5、依赖浏览器环境的 js 库不能使用，因为是 JSCore 执行的，没有 window、document 对象。

6、WXSS 中无法使用本地(图片、字体等)。

7、WXSS 转化成 js 而不是 css，为了兼容 rpx。

8、WXSS 不支持级联选择器。

9、小程序无法打开页面，无法拉起 APP。
:::
