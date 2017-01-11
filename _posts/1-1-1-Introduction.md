---
layout: doc
permalink: /docs/extensions/getting-started/introduction
title: 介绍
section: 入门
---

# 入门
<hr />
本教程会介绍如何在Shoutem平台上编写自定义的**Shoutem扩展**。它介绍了最重要的概念
完成后，您将有一个运行的**移动应用**，它使用**自定义扩展**和[Shoutem UI 工具包]({{ site.baseurl }}/docs/ui-toolkit/introduction)进行创建，并从 _Shoutem Cloud Storage_中进行数据存储。

这是一个完成的应用预览图片。
<p class="image">
<img src='{{ site.baseurl }}/img/getting-started/extension-preview.jpg'/>
</p>

## 什么是扩展？
如下图所示，每个Shoutem应用程序都是由扩展进行创建的。

<p class="image">
<img src='{{ site.baseurl }}/img/getting-started/apps-are-made-of-extensions.png'/>
</p>

扩展是一个个连接应用程序的小功能。 应用程序所有者通过Shoutem Builder将所需的扩展连接到应用程序的主导航界面，并填补好内容。 Shoutem准备了一个**开源**扩展，你可以轻松地根据您的需要进行定制。

<p class="image">
<img src='{{ site.baseurl }}/img/getting-started/shoutem-extensions.png'/>
</p>

## 关于技术

<hr />

Shoutem使用[React](https://facebook.github.io/react/)和[React Native](https://facebook.github.io/react-native/)作为构建跨平台应用程序的框架。 
React是一个开源JavaScript库，它提供了一个M**V**C模式的**View**，而React Native将iOS和Android**原生**组件暴露给React环境。 
我们还使用[JSX](https://facebook.github.io/react/docs/jsx-in-depth.html)作为使用UI组件的更简单的方法。 
在JSX中做一个界面看起来像这样：

<p class="image">
<img src='{{ site.baseurl }}/img/getting-started/jsx-component-example.png'/>
</p>

在React和React Native之上，我们使用[Redux](http://redux.js.org/) - 一个用于简化状态管理的库。 
Redux中有几个构造在下一张图片中解释。

<p class="image">
<img src='{{ site.baseurl }}/img/getting-started/redux.png'/>
</p>

每个应用程序都有一个[Redux状态管理器](http://redux.js.org/docs/basics/Store.html)，它保存着**应用程序状态**。 当状态改变时，React自动更新UI组件。 
应用程序可以在触发事件的信息（例如点击按钮）的进行触发[Redux 操作](http://redux.js.org/docs/basics/Actions.html)。 
分派的操作将由[Redux reducers](http://redux.js.org/docs/basics/Reducers.html)处理，它指定给定操作的下一个状态。 一旦reducers处理动作，应用程序将进入新状态，React会自动更新UI组件以反映新状态。

## 创建扩展
理解扩展能力的最好方式，是让你亲自进行操作。 我们从[设置您的开发环境]({{ site.baseurl }}/docs/extensions/getting-started/development-environment)开始吧！
