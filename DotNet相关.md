# .Net相关

## 使用VSCode创建.net项目

1.安装c#扩展

2.创建

```
dotnet new console
```

创建控制台应用模板，使用默认的.net框架

```
dotnet new console --framework net8.0 --use-program-main
```

带参数的创建命令

指定.net版本 使用program main作为启动入口

3.运行

```
dotnet run
```

## VSCode调试

### launch.json

1.  type：调试器类型，例如node，coreclr
2.  request：此启动配置的请求类型 目前支持launch和attach
3.  name：调试启动配置的名称
4.  presentation：具有order,group,hidden属性，可以在调试配置下拉菜单中对配置进行排序，分组和隐藏
5.  preLanuchTask：在调试会话开始之前开始任务，需要与tasks.json中的指定任务标签同名 或者设置为`${defaultBuildTask }`
6.  `program` - 启动调试器时要运行的可执行文件或文件
7.  `args` - 传递给要调试的程序的参数
8.  `env` - 环境变量（值 `null` 可用于“取消定义”变量）
9.  `envFile` - 包含环境变量的 dotenv 文件的路径
10.  `cwd` - 用于查找依赖项和其他文件的当前工作目录
11.  `port` - 附加到正在运行的进程时的端口
12.  `stopOnEntry` - 程序启动时立即中断
13.  `console` - 要使用的控制台类型，例如 `internalConsole`、`integratedTerminal` 或 `externalTerminal`



### tasks.json

任务系统，调用外部的自动化工具，例如代码检查，构建，打包，测试，部署软件系统等

1. **label**: 任务在用户界面中使用的标签。
2. **type**: 任务的类型。对于自定义任务，这可以是 `shell` 或 `process`。如果指定了 `shell`，则该命令将被解释为 shell 命令（例如：bash、cmd 或 PowerShell）。如果指定了 `process`，则该命令将被解释为要执行的进程。
3. **command**: 要执行的实际命令。
4. **windows**: 任何特定于 Windows 的属性。当该命令在 Windows 操作系统上执行时，将使用此属性代替默认属性。
5. **group**: 定义任务所属的组。在此示例中，它属于 `test` 组。属于 test 组的任务可以通过从 **命令面板** 运行 **运行测试任务** 来执行。
6. **presentation**: 定义如何在用户界面中处理任务输出。在此示例中，显示输出的集成终端 `always` 会被显示出来，并且每次任务运行时都会创建一个 `new` 终端。

## 官方

[vscode构建.net项目官方文档](https://learn.microsoft.com/zh-cn/dotnet/core/tutorials/with-visual-studio-code?pivots=dotnet-8-0)

[Launch的官方简介](https://vscode.js.cn/docs/editor/debugging)

[Tasks的官方简介](https://vscode.js.cn/docs/editor/tasks)



# Vfox相关

对于unity3d中使用的dotnet版本

- 在vscode中，暂无法使用vfox对dotnet版本进行管理
- vscode中需要使用c# Dev Kit工具包，该工具包不识别vfox设置的全局变量
- 无法获取到全局的dotnet版本

解决方案：使用官方的dotnet版本进行安装

[官方dotnet版本下载地址](https://dotnet.microsoft.com/zh-cn/download/dotnet/thank-you/sdk-8.0.404-windows-x64-installer)