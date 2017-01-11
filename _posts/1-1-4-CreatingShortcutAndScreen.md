---
layout: doc
permalink: /docs/extensions/getting-started/shortcut-and-screen
title: 创建快捷方式和屏幕
section: 入门
---

# 创建快捷方式和屏幕
<hr />

了解快捷方式的最简单方法是将它们视为**链接**到扩展的开始屏幕。 
这些链接将用于从应用程序的任何部分导航到您的扩展。 
扩展程序可以显示多个快捷方式。 让我们现在创建一个。

```ShellSession
$ shoutem shortcut add openRestaurantsList
Enter shortcut information.
Title: Restaurants
Description: Enable users to browse through list of restaurants

`openRestaurantsList` shortcut is created.
```

你的`extension.json`文件刚刚修改了：

```json{6-10}
#file: extension.json
{
  "name": "restaurants",
  "version": "0.0.1",
  "title": "Restaurants",
  "description": "List of restaurants",
  "shortcuts": [{
    "name": "openRestaurantsList",
    "title": "Restaurants",
    "description": "Enable users to browse through list of restaurants"
  }]
}
```

让我们现在添加一个屏幕。 屏幕是代表移动屏幕的React组件。

### 创建列表屏幕

创建一个新的屏幕：

```ShellSession
$ shoutem screen add RestaurantsList
File `app/screens/RestaurantsList.js` is created.
```

屏幕定义附加到extension.json。

```json{11-13}
#file: extension.json
{
  "name": "restaurants",
  "version": "0.0.1",
  "title": "Restaurants",
  "description": "List of restaurants",
  "shortcuts": [{
    "name": "openRestaurantsList",
    "title": "Restaurants",
    "description": "Allow users to browse through list of restaurants"
  }],
  "screens": [{
    "name": "RestaurantsList"
  }]
}
```

Shoutem CLI还创建了`app/screens/`文件夹与`RestaurantsList.js`文件：

```javascript
#file: app/screens/RestaurantList.js
import React, {
  Component
} from 'react';
import {
  Text
} from 'react-native';

export default class RestaurantsList extends Component {
  render() {
    return (
      <Text>Hello World!</Text>
    );
  }
}
```

在React中，`Component`在`render`方法中指定它的UI。 
现在当屏幕创建时，我们需要将其连接到extension.json中的shortcut。

```json{10}
#file: extension.json
{
  "name": "restaurants",
  "version": "0.0.1",
  "title": "Restaurants",
  "description": "List of restaurants",
  "shortcuts": [{
    "name": "openRestaurantsList",
    "title": "Restaurants",
    "description": "Allow users to browse through list of restaurants",
    "screen": "@.RestaurantsList"
  }],
  "screens": [{
    "name": "RestaurantsList"
  }]
}
```

注意，`shortcuts`中的对象有`name`属性，它标识它和`screen`，它代表当轻击shortcut时要打开的screen。 
在`name`属性中，使用**relative name**来定义扩展部分。 
在像`screen`这样的属性中，引用了一些扩展部分，使用**absolute name**。 
扩展部分的绝对名称遵循以下结构：`{developerName}.{extensionName}.{extensionPartName}`。 
但是，对于当前扩展的部分，可以简单地使用`@.{extensionPartName}`。 字符`@.`替换您的`{developerName}.{extensionName}.`。

## 导出扩展部分

应用程序需要知道在哪里可以找到扩展部件。  
为了让你自由地为你的扩展使用任何文件夹结构，我们希望你的`app`文件夹包含名为`index.js`的文件，它导出所有的扩展部分，例如：

- screens,
- reducer,
- actions,
- middleware and
- application lifecycle methods.

我们不会使用本教程中的最后三个，但是你可以在[这里](/docs/coming-soon)找到更多信息。 当前的`index.js`看起来如下：

```JSX
#file: app/index.js
// Constants `screens` and `reducer` are exported via named export
// It is important to use those exact names

export const screens = {};

export const reducer = {};
```

导出已经创建好的screen。

```javascript{1,4}
#file: app/index.js
import RestaurantsList from './screens/RestaurantsList.js'

export const screens = {
  RestaurantsList
};

export const reducer = {};
```

更新你的扩展：

```ShellSession
$ shoutem push
Uploading `Restaurants` extension to Shoutem...
Success!
```

在[Shoutem 构建器](/docs/coming-soon)中点击`Screens`，然后点击`+`。 
你终于可以看到你的`Restaurants`扩展。 如果扩展程序有更多shortcuts，则可以悬停在扩展程序上方时看到它们。

<p class="image">
<img src='{{ site.baseurl }}/img/getting-started/add-modal-shortcut.png'/>
</p>

点击`Restaurants`扩展名。 Shotcut将会插入到应用导航中。

<p class="image">
<img src='{{ site.baseurl }}/img/getting-started/extension-hello-world.png'/>
</p>

在预览中，您可以看到_Hello World_屏幕。 让我们在屏幕上放一些UI组件。
