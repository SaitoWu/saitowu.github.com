---
layout: post
title: The Architecture of F2E
category: note
excerpt: showing off linner
---

前端架构
=======

## 总述

在互联网应用越来越大，越来越复杂的今天，我们不可避免的需要工具来管理我们的前端代码。
替代以前的一个巨大的脚本文件，我们希望可以将文件写入不同的文件模块。并且希望代码可以
重用，可以简单的引用和添加各种各样的依赖到我们的项目（ 无论是菜单一样的 UI 组件还是
一个类似 jQuery 的 DOM 操作库）。不止是 JavaScript 我们希望可以用这种方式来组织，
他应该也包含 CSS，HTML 模板，字体，图片和其他静态文件。

#### 为什么前端需要模块化开发

随着互联网应用越来越大，前端的开发也越来越复杂。如果还维持在以往以页面为单位的开发，
会导致很多问题，类似依赖管理，命名冲突等棘手的问题。

命名冲突是最常见的问题：

```javascript
// util.js
function log(message) {

}
// logger.js
function log(message) {

}
```

当页面的 `script` 标签同时依赖这两个文件时便会产生冲突，导致后面函数会覆盖前面的。
从而可能会产生一些预想之外的结果。

而传统的解决方案是使用命名空间：

```javascript
// util.js
var util.log = function(message) {

}

// logger.js
var logger.log = function(message) {

}
```
这样会带来显而易见的问题，所有的代码会变得冗余且编写困难。

如果使用模块化的编写方案，例如 Common Module Definition，代码见的依赖会变得格外简单。

```javascript
// util.js
var log = function(message) {

}

module.exports = log

// logger.js
var log = function(message) {

}

module.exports = log

// app.js using util.js log for logging
var log = require("util.js")

log("Hello Module Definition")
```

`util.js` 与 `logger.js` 不会相互冲突，他们会被工具包装为 CMD 下的定义方式。
然后通过依赖的方式来解决冲突

依赖管理同样是一个棘手的问题：
```html
<script src="/util.js" type="text/javascript"></script>
<script src="/logger.js" type="text/javascript"></script>
<script src="/app.js" type="text/javascript"></script>
```
如果你有一大堆的依赖关系，你必须依序将所有的文件引入，否则会导致变量未定义等问题。
如果使用了 Common Module Definition 便可以无序的引入组件文件。

在样式方面由于 CSS 与浏览器本身的限制我们仍然无法使用这样的技术来划分模块。

```css
/* layout.css */
.box {

}

/* dropdown.css */
.box {

}
```

这样的话 dropdown.css 的 .box 便会覆盖之前的 .box 的样式，所以我们需要在模块中添加限制。

所以在样式表方面我们需要详细的划分模块的命名空间，例如在 dropbox.css 中我们可以用这样的命名方式 .dropdown .box 的方式。
这边便不会影响到父级的样式。

#### 如何进行模块化开发

首先对页面进行模块化的拆分，将模块的定义文件放在一个文件夹下，其中包含有
JavaScript CSS 以及 Templates（前后端）的文件。模块的脚本文件默认会被 CMD 包装，
而 CSS 文件，开发着需要对文件进行特殊的命名，默认以模块文件夹名为命名空间，
以防与别人冲突，而模板文件则不会有这样的顾虑。

我们开发了 Linner 来支持模块化的开发。

## 架构概览

![](http://cl.ly/image/1Y1U0v0m2c1C/Image%202014-04-07%20at%209.36.05%20PM.png)

在前端项目中，我们在底层拥有基础组件库与可视化编辑框架用于装修需求。
在基础组件之上，我们会沉淀出标准的组件库，类似电商标准组件库之类的类库，
而真正的类库中开发者也需要将自己的组织为组件的形式。

而工具则为整个过程保驾护航。

## 工具阐述

简单来说，Linner 允许我们做：

1. 项目结构化
2. 页面组件化
3. 仓库管理
4. 使用 CoffeeScript 与 SCSS 来替代 JavaScript 与 CSS
5. 合并 CSS 与 JavaScript 文件
6. 复制 CSS 与 JavaScript 文件
7. 预编译 CSS 与 JavaScript 文件
8. 合并图片至一张大图（Sprites）
9. 压缩 CSS 与 JavaScript 文件
10. 监视文件系统变化并实时更新

#### 1. 项目结构化

项目通过标准化的方式来组织：

```bash
.
├── app
│   ├── components
│   │   └── dropdown
│   │       ├── view.coffee
│   │       ├── view.hbs
│   │       └── view.scss
│   ├── images
│   │   └── logo.png
│   ├── scripts
│   │   └── app.coffee
│   ├── styles
│   │   └── app.scss
│   ├── templates
│   │   └── welcome.hbs
│   └── views
│       └── index.html
├── bin
│   └── server
├── config.yml
├── public
├── test
└── vendor
```
`app` 文件夹是用户自己编写代码的地方
  * `images` 用以存放项目相关的图片文件
  * `scripts` 用以存放项目相关的 JavaScript 文件
  * `styles` 用以存放项目相关的 Stylesheet 文件
  * `templates` 用以存放项目相关的前端模板文件
  * `views` 用以存放项目相关的后端模板文件
  * `components` 用以存放项目的组件文件

`config.yml` 是整个项目的配置文件

`bin` 文件夹可以让用户很方便的启动一个本地的服务器，以当前文件夹作为根

`test` 文件夹可以使用户编写一些单元测试来测试自己的前端项目

`vendor` 文件夹可以使用户引入第三方的代码组件，如 jQuery、Underscore 等

`public` 文件夹是项目打包后文件位置，发布项目所需要的所有文件

#### 2. 页面组件化

> 不要面向页面编程，要面向组件编程。

当拿到网站的整体设计稿时，我们应该首先去找出页面间有相同逻辑的模块。
将他们抽出，考虑如何将其设计为可复用的模块。

我们可以将模块的内容包含 JavaScript CSS 与前后端模板组织在 `app/components`
内，通过在 `app/scripts` 里面的 `app.coffee` 中去初始化所有的组件。

通过 `cmd` 的形式去管理组件与组件之间的依赖关系。这样组件内部就可以通过
`module.exports = "dropdown"` 与 `require "dropdown"` 这样的形式去导出与依赖组件。

#### 3. 仓库管理

通过 `bundles` 来管理远端依赖，在项目中可以非常方便的引入一些著名的第三方库。
如 jQuery，Underscore 等。

依赖可以规定多个版本，当需要升级版本时，可以更改 `version` 与 `url`
在工具下次启动时便可以拉取新版本的依赖。如果希望依赖一直为最新状态，可以将 `version` 设置为 `master` 这样工具会在每次启动时都获取最新的文件。

#### 4. 使用 CoffeeScript 与 SCSS 来替代 JavaScript 与 CSS

##### CoffeeScript

CoffeeScript 是这一门编程语言构建在 JavaScript 之上，其被编译成高效的 JavaScript，
这样你就可以在 Web 浏览器上运行它，或是通过诸如用于服务器端应用的 Node.js 一类的技术来使用它。
编译过程通常都很简单，产生出来的 JavaScript 与许多的最佳做法都保持了一致。

##### SCSS

SCSS 扩展了 CSS3，增加了规则、变量、混入、选择器、继承等等特性。
SCSS 生成良好格式化的 CSS 代码，易于组织和维护。

##### 使用成本

使用 CoffeeScript 与 SCSS 可以大幅降低开发成本，使用 CoffeeScript 可以避免一些常见的 JavaScript 开发错误。
而 SCSS 则可以更好的抽象样式文件，使样式得到更好的维护。并且 CoffeeScript 与 SCSS 的学习成本都很低，
前端可以通过很简短的学习就能立刻写出优雅的代码。

#### 5. 合并 CSS 与 JavaScript 文件

前端文件的合并可以明显的减少 HTTP 请求，明显的加快网页的浏览速度。

#### 6. 复制 CSS 与 JavaScript 文件

复制文件是一个很普遍的需求，可以将文件从一个位置复制到另一个位置。如果是 CoffeeScript 文件或者 SCSS 文件，工具会帮助转换为对应的 Javascript 与 CSS 文件。

#### 7. 预编译 JavaScript 模板文件

随着互联网应用越来越大，前端模板的需求也日渐突出。在使用前端模板的过程中，
为了提高前端的渲染性能，我们需要对前端模板进行预编译。预编译的结果是使 `templates`
文件能直接转化为 JavaScript 的方法调用。这样可以以非常快的速度来渲染前端模板。

#### 8. 合并图片至一张大图（Sprites）

当页面内的图片很多时，会产生多个 HTTP 请求，当请求变多时会严重影响网站的速度。
所以我们需要将多个 PNG 图片合并成一张图片。同时利用 CSS 的 `background`
来显示对应的单个图片

#### 9. 压缩 CSS 与 JavaScript 文件

在项目发布上线时需要将资源文件进行最大程度的压缩。从而减少 HTTP 请求文件的体积。
从而可以将文件尽快的传输给用户，使页面更快的展示出来。

工具提供了快速的文件名的版本替换，可以使服务器更好的缓存压缩后的文件。

#### 10. 监视文件系统变化并实时更新

文件系统的实时监控可以监控到项目内文件的变动，同时重新执行整个工具的逻辑。

在开发阶段，我们可以尽最大程度的提高开发者的效率。例如使用浏览器实时刷新。
当文件系统有任何变化时，工具会发动 LiveReload 来自动刷新页面，
当用户只修改了 CSS 文件时，我们甚至可以不刷新页面，直接重载 CSS 文件。
极大的提高开发效率。

## 性能调优

大约 80%-90% 的终端响应时间是花费在前端，其中包含下载页面中的图片，样式表，脚本，flash等。Yahoo 为此总结了 14 条规则，成为网站性能优化的事实标准。

#### 雅虎网站性能优化的 14 条规则：

1. 尽可能减少 HTTP 请求数
2. 使用 CDN（内容分发网络）
3. 为文件头指定 Expires 或 Cache-Control，使内容具有缓存性
4. 使用 Gzip 压缩内容
5. 把 CSS 放到顶部
6. 把 JavaScript 放在底部
7. 避免在 CSS 中使用 Expressions
8. 把 JavaScript 和 CSS 都放到外部文件中
9. 减少 DNS 查找次数
10. 压缩 JavaScript 和 CSS
11. 避免重定向
12. 剔除重复的 JavaScript 和 CSS
13. 配置 Etags
14. 使 AJAX 缓存

#### 对规则的分析：

1. 代码编写方面的规则：
  * 把 CSS 放到顶部
  * 把 JavaScript 放在底部
  * 把 JavaScript 和 CSS 都放到外部文件中
  * 避免在 CSS 中使用 Expressions
  * 使 AJAX 缓存
2. 打包方面的规则：
  * 尽可能减少 HTTP 请求数
  * 压缩 JavaScript 和 CSS
  * 剔除重复的 JavaScript 和 CSS
3. 部署方面的规则：
  * 使用 CDN（内容分发网络）
  * 为文件头指定 Expires 或 Cache-Control，使内容具有缓存性
  * 使用 Gzip 压缩内容
  * 减少 DNS 查找次数
  * 避免重定向
  * 配置 Etags

#### 对规则的实践

1. 部署方面的规则，应用 Nginx 为静态文件添加 Expires 跟 Cache-Control 头，
配置 Etags，并启用 Gzip 压缩。并且避免在 Nginx 中做重定向，有条件的话可以
启用 CDN，并优化网络配置以减少 DNS 查找次数。
2. 代码编写方面的规则，需要在编写代码种形成规范。默认使用类似 jQuery 这样的库
便可以对 AJAX 进行缓存。
3. 打包方面 Linner 可以合并 JavaScript 与 CSS 文件, 并且支持小图片的合并，
用以减少 HTTP 请求数。同时 Linner 的仓库管理可以避免重复的 JavaScript 与 CSS
文件的出现。在 `build` 过后所有的文件将会被压缩。


## 附件

#### 工具的使用

* 安装 Ruby 2.0.0 以上版本

* 安装 Linner 及其使用规则

```bash
# 安装 Linner
gem install linner

# 使用 Linner 创建项目结构
linner new webapp && cd webapp

# 在项目下启动 Linner
linner watch

# 退出 Linner
CTRL + C

# 打包资源文件
linner build

# 清空打包的资源文件
linner clean
```

#### config.yml 文件配置详解

```yaml
paths:
  app: "app"
  test: "test"
  public: "public"
groups:
  scripts:
    paths:
      - app/scripts
    concat:
      "/scripts/app.js": "app/**/*.{js,coffee}"
      "/scripts/vendor.js": "vendor/**/*.{js,coffee}"
    order:
      - vendor/jquery-1.10.2.js
      - ...
      - app/scripts/app.coffee
  styles:
    paths:
      - app/styles
    concat:
      "/styles/app.css": "app/styles/**/[a-z]*.{css,scss,sass}"
  images:
    paths:
      - app/images
    sprite:
      "../app/images/icons.scss": "app/images/**/*.png"
  views:
    paths:
      - app/views
    copy:
      "/": "app/views/**/*.html"
  templates:
    paths:
      - app/templates
    precompile:
      "/scripts/templates.js": "app/templates/**/*.hbs"
modules:
  wrapper: cmd
  ignored: vendor/**/*
  definition: /scripts/app.js
sprites:
  url: /images/
  path: /images/
  selector: .icon-
revision: index.html
notification: true
bundles:
  jquery.js:
    version: 1.10.2
    url: http://code.jquery.com/jquery-1.10.2.js
  handlebars.js:
    version: 1.0.0
    url: https://raw.github.com/wycats/handlebars.js/1.0.0/dist/handlebars.runtime.js
```

`paths` 表示当前工具做监视的文件系统目录

`groups` 区分了不同的组，每个组可以有一个名字。在组内部的声明中需要指定当前组的
`paths`，然后可以指定一系列的操作原语，包括：`concat` `order` `copy`
`precompile` `sprite` 等

`modules` 定义了需要被 `CMD` 包装的文件路径，以及包装定义的头文件连接位置

`sprites` 定义了图片 `sprites` 的一些生成规则，例如以 `.icon-` 开头来生成 CSS
列表，这样用户便可以以这样的 CSS 选择器来直接生成样式。

`revision` 定义了需要被加载 `rev` 的文件，用以 md5 的文件名来替换旧文件名

`notification` 定义了是否需要有通知系统，（用以 Mac 系统的通知）

`bundles` 定义了项目的依赖关系，项目可以依赖很多第三方的项目，可以自定义版本号。
如果需要每次启动都更新最新版本的依赖，可以将 `version` 设置为 `master`
