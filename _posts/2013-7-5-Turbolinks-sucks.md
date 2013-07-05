---
layout: post
title: Turbolinks sucks
category: note
excerpt: also sprockets
---

### Turbolinks 的出现是为了解决什么问题?

- 减少 Javascript 与 CSS 编译时间.

### 解决的同时送给你了什么新的礼物?

- 潜在的内存泄露 ( Chrome 的 DOM 与 JS Object 之间很容易泄露内存, 变相提高了程序员素质.

- 额外的适配原有后端逻辑的代码. ( 例如 适配 redirect_to 301, 在 turbolinks 代码内部. cache 以及 cookie

- 前端代码的 ready 事件改写. ( page:load 可以用 jquery.turbolinks 继续监听 $.ready

- 前端监听事件的复杂化, 本来就一个 DOM ready, 现在变成了 5 个事件. 你可以小心翼翼的来处理这 5 个事件, 来保证程序的正常运行.

- `$(document).on` 以及老版本 jQuery 的 `$(".class").live` 要特别小心. 尽量使用 BackBone 之类的框架, 不要面向 DOM 编程. 因为事件绑定之类的会随着 View 对象的消失而被清理, 不然需要处理很多 `on`, `off`, 以及潜在的多次事件绑定.

### 怎么样的 Turbolinks 才是对的?

Turbolinks 站在 Server Render 跟 Client Render 的十字路口, 决定适配这个世界. 我不认为这是未来的开发方向..

Turbolinks 里面有用的功能, 不用几行就实现了, 而多余的很多行以及后端的代码, 都是为了适配写的, 解决了一些纯前端框架根本不会遇到的问题. 而且顺利的提高了使用 Turbolinks 的学习曲线.

我个人很喜欢 yehuda 的 Ember.js 的宣传语, Ambitious Web Applications. 我觉得未来的体验需要有野心的前端框架来解决. 而不是 Turbolinks.

### 而 Sprockets 的出现是为了解决什么问题?

- 连接 ( 其他都是附带的.

### 前端开发到底需要的是什么?

难道真的需要一个打包工具? 据我所知, 在荒蛮的开发大地上, 我们的前端朋友们用 Ant 也打了不少年的包了, 这东西真不稀奇, 打包不是他们的痛点.

前端开发的痛点是组织代码, 怎么写都不爽才是问题. 因为没有标准的模块化. 所以大家折腾各种 CMD/AMD 的标准. 这下写的爽了.. 也没 Sprockets 什么事了. 因为 module 天然有 require 的属性, 所以必然是通过 module 来判断哪些文件被引用了, 然后一起打包. Sprockets 帮不上什么忙.

而 SASS 则更是没 Sprockets 什么事, 因为 SASS 天然的支持 partials, 默认就是这样 build 在一起的.

所以 Sprockets 在现在的开发模式下, 有用的功能几乎为 0 . ( 不过在当时这些 模块定义 AMD/CMD 还没有被大家广泛接受的时候是有用的, 不过仅限于 Javascript.

Sprockets 希望你将 Javascript lib 打包成 Gem 来做管理, 本来就是一个 Anti Pattern. 因为这与开发阶段完全脱离, 非常依赖第三方人员升级 Gem. lib 与 作者结合的发布方式才是对的. 例如 bower ( 方向对了, 但是实现的不好.

### TIL

感谢 [@Hooopo](https://twitter.com/Hooopo) 纠正关于 Turbolinks 与浏览器连接数的问题.

Click links will not fire conditional GET, command + r will !
