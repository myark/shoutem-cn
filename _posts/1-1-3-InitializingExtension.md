---
layout: doc
permalink: /docs/extensions/getting-started/initializing-extension
title: 配置扩展
section: 入门
---

# 配置扩展
<hr />

这里是我们在[简介]({{ site.baseurl }}/docs/getting-started/introduction)中显示的餐馆扩展的样机（我也不知道怎么翻译..）。 我们正在构建_Restaurants_扩展。

<p class="image">
<img src='{{ site.baseurl }}/img/getting-started/extension-preview.jpg'/>
</p>

左应用程序界面列出restaurants后，右边的显示一个特定的restaurant详细信息。 

让我们开始创建吧！找到您要保留扩展文件的文件夹：

```ShellSession
$ mkdir Restaurants && cd Restaurants
```

使用基本扩展数据进行初始化扩展。

```ShellSession
$ shoutem init restaurants
Enter information about your extension. Press `return` to accept (default) values.
Title: (Restaurants)
Version: (0.0.1)
Description: List of restaurants

Extension initialized!
```

这些信息将存储在`extension.json`文件中。

> #### 注意
> 如果你不记得一些命令的结构，输入：`shoutem -h`或`shoutem <command> -h`，你应该用[CLI commands]({{ site.baseurl }}/docs/extensions/reference/cli)

## 目录结构
初始化过程将为您生成骨架文件夹和文件结构。 您的结构如下所示：

```
Restaurants/
  ├ app/
  |  ├ const.js
  |  ├ index.js
  |  └ package.json
  ├ server/
  |  └ package.json
  └ extension.json
```

让我们描述一下结构：

- `app/`: 应用代码
- `server/`: 服务端代码
- `extension.json`: 扩展的基础信息

具体部分稍后会进行描述。

在`extension.json`文件中你可以看到：

```JSON
#file: extension.json
{
  "name": "restaurants",
  "version": "0.0.1",
  "title": "Restaurants",
  "description": "List of restaurants"
}
```

`name`属性，结合您的开发人员名称，唯一标识该扩展：`developer.restaurants`。我们将使用属性`name`来定义扩展部分。

扩展现在只在您的计算机上本地可用。 我们需要将其上传到Shoutem，以便您可以在应用程序中安装它。

```ShellSession
$ shoutem push
Uploading `Restaurants` extension to Shoutem...
Success!
```

要测试我们的扩展，我们需要在Shoutem应用程序中安装它。 
您可以在[Shoutem 构建器](/docs/coming-soon)中创建新应用程序，或在新安装应用的命令中带上`--new`选项。

```ShellSession
$ shoutem install --new Restaurants
Extension is installed onto newly created `Restaurants` application.
See it in browser: `https://builder.shoutem.com/apps/52634`
```

转到[Shoutem 构建器](/docs/coming-soon)中的`Extensions`选项卡。您将看到您的应用程序上安装了该扩展程序。

<p class="image">
<img src='{{ site.baseurl }}/img/getting-started/extension-tab-extension.png'/>
</p>

然而，当你现在进入`Screen`选项卡，然后点击`Screen`旁边的`+`，这个扩展将不会显示。

<p class="image">
<img src='{{ site.baseurl }}/img/getting-started/add-content-no-extension.png'/>
</p>

这是因为`Add screen` 模态框只显示_shortcuts_的扩展。 我们需要在扩展中创建快捷方式。
