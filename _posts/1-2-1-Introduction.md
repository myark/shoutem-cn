---
layout: doc
permalink: /docs/ui-toolkit/introduction
title: 介绍
section: UI 工具包
---

# 介绍

![UI 工具包]({{ site.baseurl }}/img/ui-toolkit/introduction@2x.jpg "UI toolkit"){:.docs-component-image}

Shoutem UI工具包使您能够轻松构建专业的React Native应用程序。

它由三个库组成：

- [@shoutem/ui](https://github.com/shoutem/ui): 漂亮和可定制的UI[组件](http://shoutem.github.io/docs/ui-toolkit/components/typography)
- [@shoutem/theme](https://github.com/shoutem/theme): “使用CSS的方式”来完成整个应用的[主题](http://shoutem.github.io/docs/ui-toolkit/theme/introduction)
- [@shoutem/animation](https://github.com/shoutem/animation): 声明式应用现成的[动画](http://shoutem.github.io/docs/ui-toolkit/theme/animation/introduction) 


## 先决条件
开始之前，请确保您有：

- 安装好[node](https://nodejs.org/en/)
- 安装好[npm](https://www.npmjs.com/)
- 安装好[React Native](https://facebook.github.io/react-native/docs/getting-started.html)

## 安装

创建一个新的React Native项目：

```bash
$ react-native init HelloWorld && cd HelloWorld
```

在项目中安装并连接`@shoutem/ui`模块：

```bash
$ react-native install @shoutem/ui
$ react-native link
```

要检查我们在UI工具包中有哪些组件，只需将以下内容复制到React Native项目的`index.ios.js`文件中：

```JavaScript
#file: index.ios.js
import React, { Component } from 'react';
import { AppRegistry } from 'react-native';
import { Examples } from '@shoutem/ui';

class HelloWorld extends Component {
  render() {
    return (
      <Examples />
    );
  }
}

AppRegistry.registerComponent('HelloWorld', () => HelloWorld);
```

最后，运行起你的应用吧！

```bash
$ react-native run-ios
```

要查看其他组件，只需从`@ shoutem/ui`导入它们并进行渲染。

您还可以在布局中的任何位置使用标准React Native组件，但它们不会继承主题或父样式，因此您需要手动为它们设置样式。


## 样式名称

样式名称类似于CSS类。 一旦应用特定的`styleName`，它将继承在Theme中定义的样式规则。

本节涵盖可用于多个UI工具包组件及其变体的常用样式名称。


### 基于大小的样式名称变体
* **sm-gutter** : 小尺寸，默认为5px。
* **md-gutter** : 中等大小，默认为15px。
* **lg-gutter** : 大尺寸，默认为30px。
* **xl-gutter** : 超大尺寸，默认为45px。
  
> 在`View`，`Tile`和`Overlay`组件gutter被应用为填充，而`Text`（排版）和`Button`组件gutter被应用为边距。

如果要仅将gutter应用于组件的特定边（即`右`）或垂直边，则可以通过使用下面列出的其他位置样式名称关键字来指定。    

### 基于位置的样式名称变体
* **_size_-gutter-left** : gutter将仅应用于目标组件的左侧。
* **_size_-gutter-right** : gutter将仅应用于目标组件的右侧。
* **_size_-gutter-top** : gutter将仅应用于目标组件的顶部。
* **_size_-gutter-bottom** : gutter将仅应用于目标组件的底侧。
* **_size_-gutter-horizontal** : gutter将仅应用于目标组件的水平边（左和右）。
* **_size_-gutter-vertical** : gutter将仅应用于目标组件的垂直边（顶部和底部）。 

#### 圆角
- 此样式名称将边界半径（默认为2px）应用于目标组件。

#### flexible
- 此样式名称将flexbox应用于目标组件，以便它填充父容器组件。

#### inflexible
- 使用此样式名称，组件将根据其宽度/高度属性进行调整，但会使其完全不灵活。 

#### 可折叠
- 此样式名称会导致组件在父容器溢出时收缩。

#### 伸展
- 此样式名称使组件完全填充父容器。 
  
下面是一个示例，其中可以使用常见的样式名称：
<br />  

#### JSX 声明
```JSX
<Overlay>
  <Overlay styleName="collapsed"><Heading>-20%</Heading></Overlay>
  <Title styleName="md-gutter-top">COOL BLACK AND WHITE STYLISH WATCHES</Title>
  <Subtitle styleName="line-through sm-gutter-top">$280.00</Subtitle>
  <Heading>$250.00</Heading>
  <Button styleName="md-gutter-top"><Icon name="cart" /><Text>ADD TO BASKET</Text></Button>
</Overlay>
```
