---
layout: doc
permalink: /docs/extensions/getting-started/development-environment
title: 设置环境
section: 入门
---

# 设置环境
<hr />

在开始创建一个扩展之前，请先设置好开发环境。
在命令行下安装Shoutem[命令行界面](https://www.npmjs.com/package/@shoutem/cli) (CLI)，通过`npm`工具即可：

```ShellSession
$ npm install -g @shoutem/cli
``` 

> #### 注意
> 如果你没有安装npm，最好的安装方法是通过[安装](https://nodejs.org/en/download/) Node.js，它包括了npm命令。 如果上一个命令由于_权限_问题失败，您需要使用`sudo`权限运行它。

标志```-g``是全局安装CLI，所以你可以在任何文件夹中使用`shoutem`命令。

<hr />

每个Shoutem用户都可以成为Shoutem的开发人员。 如果您还没有，请转到[Shoutem](http://www.shoutem.com)并创建新帐户。 
现在您可以注册为开发人员：

```ShellSession
$ shoutem login
Enter your Shoutem credentials.
Email: developer@example.com
Password:
```

输入正确的Shoutem凭据后，系统将要求输入指定的开发人员名称。

```ShellSession

Logged in as `developer@example.com`.
Enter developer name.
Developer name: developer

Registered as `developer`.
```

## 编辑器
要编写扩展，你只需要一个编辑器就够了。使用**你所喜欢的编辑器**即可！
