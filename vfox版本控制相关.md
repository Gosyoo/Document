# vfox

## 简介

`vfox` 是一款跨平台、可拓展的通用版本管理器。支持**原生Windows**以及**Unix-like**! 通过它，您可以**快速安装和切换**开发环境。



- 支持**不同项目不同版本**、**不同Shell不同版本**以及**全局版本**

- 支持常用Shell(Powershell、Bash、ZSH),并提供补全功能



## 常用命令

- 展示所有可用的插件：
vfox available 

- 添加一个插件： 
vfox add  <sdk-name>

- 删除一个插件：
vfox remove <sdk-name>          Remove a plugin

- 更新插件：
vfox update [<sdk-name> | --all] Update a specified or all plugin(s)

- 打印一个插件的信息
vfox info <sdk-name>            Show plugin info

- 查找插件是否存在vfox中，并打印所有可下载的版本
vfox search <sdk-name>          Search available versions of a SDK

- 安装一个插件版本：
vfox install <sdk-name>@<version> Install the specified version of SDK

- 卸载一个插件版本：
vfox uninstall <sdk-name>@<version> Uninstall the specified version of SDK

- 选择一个插件版本应用与当前的shell：
vfox use [--global --project --session] <sdk-name>[@<version>]   Use the specified version of SDK for different scope

- 打印所有安装的sdk版本：
vfox list [<sdk-name>]              List all installed versions of SDK

- 打印当前正在使用的插件版本
vfox current [<sdk-name>]           Show the current version of SDK

- 帮助
vfox help                      Show this help message



## 全局唯一

```
vfox use -g nodejs
```

-g 指令可以设置该插件全局唯一的版本

默认配置在`$HOME/.version-fox/.tool-versions`文件中进行管理。

`$HOME/.version-fox/.tool-versions` 文件内容将会如下所示：

### 注：

对于**Windows**用户:

1.请确保系统环境变量`Path`中不存在**之前**通过其他方式安装的运行时!

2.`vfox` 会自动将安装的运行时添加到**用户环境变量** `Path`中。

3.如果你的`Path`中存在**之前**通过其他方式安装的运行时, 请手动删除!

[官方文档](https://vfox.lhan.me/zh-hans/guides/intro.html)

