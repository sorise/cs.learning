## [dotnet new 命令](#)
**介绍**：用于根据指定的模板，创建新的项目、配置文件或解决方案，属于第一个必学dotnet命令, [官方文档](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new)

----

### [1. new 命令](#)
命令调用 [模板引擎](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new) 如(console、web、mvc、wpf)，以根据指定的模板和选项在磁盘上创建项目。

```shell
#基本选项
dotnet new <TEMPLATE> [--dry-run] [--force] [-lang|--language {"C#"|"F#"|VB}]
    [-n|--name <OUTPUT_NAME>] [-f|--framework <FRAMEWORK>] [--no-update-check]
    [-o|--output <OUTPUT_DIRECTORY>] [--project <PROJECT_PATH>]
    [-d|--diagnostics] [--verbosity <LEVEL>] [Template options]

#帮助信息
dotnet new -h|--help

#子命令
dotnet new <sub_command>
```

#### 1.1 选项参数
* **--dry-run** 显示有关以下内容的摘要：给定命令运行导致模板创建时发生的情况。 自 .NET Core 2.2 SDK 起可用。
* **--force** 强制生成内容，即使会更改现有文件，也不例外。 当选择的模板将覆盖输出目录中的现有文件时，需要执行此操作。
* **-?|-h|--help** 打印命令帮助。 可针对 dotnet new 命令本身或任何模板调用它。 例如 dotnet new mvc --help。
* **-lang|--language {C#|F#|VB}** 要创建的模板的语言。 接受的语言因模板而异（请参阅参数部分中的默认值）。 对于某些模板无效
> 某些 shell 将 # 解释为特殊字符。 在这些情况下，请将语言参数值括在引号中。 例如 dotnet new console -lang "F#"。
* **-n|--name \<OUTPUT_NAME\>** 所创建的输出的名称。 如果未指定名称，使用的是当前目录的名称。
* **-f|--framework \<FRAMEWORK\>** 指定目标框架。 它需要目标框架名字对象 (TFM),[参数可以参考](https://learn.microsoft.com/zh-cn/dotnet/standard/frameworks)。 示例：“net6.0”、“net7.0-macos”。 此值将反映在项目文件中。
  * net8.0
  * net7.0
  * netstandard2.1
  * netcoreapp3.1
  * netcore451 [win81]
* **-no-update-check** 禁止在实例化模板时检查模板包更新。 自 .NET SDK 6.0.100 之后可用。 从使用 dotnet new --install 安装的模板包实例化模板时，dotnet new 会检查模板是否有更新。 从 .NET 6 开始，不对 .NET 默认模板进行更新检查。 若要更新 .NET 默认模板，请安装 .NET SDK 的修补程序版本。
* **-o|--output \<OUTPUT_DIRECTORY\>** 用于放置生成的输出的位置。 默认为当前目录。
* **--project \<PROJECT_PATH\>** 模板添加到的项目。 此项目用于上下文评估。 如果未指定，将使用当前目录或父目录中的项目。 自 .NET SDK 7.0.100 之后可用。
* **-d|--diagnostics** 启用诊断输出。 自 .NET SDK 7.0.100 之后可用。
* **-v|--verbosity \<LEVEL\>** 设置命令的详细级别。 允许的值为 q[uiet]、m[inimal]、n[ormal] 和 diag[nostic]。 自 .NET SDK 7.0.100 之后可用。
#### 1.2 TEMPLATE 参数
调用命令时要实例化的模板。 **每个模板可能具有可传递的特定选项**。 有关详细信息，[请参阅模板选项](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new)。

可以运行 dotnet new list 以查看所有已安装模板的列表。

```shell
dotnet new list
```

|模板| 	短名称                                                                                               |	语言|	Tags|	已引入|
|:----|:---------------------------------------------------------------------------------------------------|:----|:----|:----|
|控制台应用程序| 	[console](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new-sdk-templates#console)   |	[C#]、F#、VB|	常用/控制台	|1.0|
|类库	| [classlib](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new-sdk-templates#classlib)	 |[C#]、F#、VB	|常用/库	|1.0|
|WPF 应用程序| 	[wpf](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new-sdk-templates#wpf)	          |[C#]、VB	|常用/WPF|	3.0（对于 VB，则为 5.0）|
|ASP.NET Core Web API| 	[webapi](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new-sdk-templates#webapi)     |	[C#]，F#	|Web/Web API/API/Service/WebAPI|	1.0|
|ASP.NET Core gRPC 服务| 	[grpc](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new-sdk-templates#web-others)   |	[C#]	|Web/gRPC|	3.0|
|含 React.js 的 ASP.NET Core| 	[react](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new-sdk-templates#spa)         |	[C#]|	Web/MVC/SPA	|8.0|
|ASP.NET Core Web 应用程序 (Model-View-Controller)| 	[mvc](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new-sdk-templates#web-options)	  |[C#]，F#|	Web/MVC|	1.0|
#### 1.3 例子
**在当前目录中创建 F# 控制台应用程序项目：** ,并且不使用顶层语句。
```shell
dotnet new console --language "C#" --use-program-main
```

在当前目录中新建没有设置身份验证的 ASP.NET Core C# MVC 项目：
```shell
dotnet new mvc -au None --language "C#" --framework "netcoreapp3.1"
```
在当前目录中创建 global.json ，将 SDK 版本设置为 8.0.101：
```shell
dotnet new globaljson --sdk-version 8.0.101
```

### [2. new 子命令](#)
从 .NET 7 SDK 开始，dotnet new 语法已更改：

* --list、--search、--install 和 --uninstall 选项已变更为 list、search、install 和 uninstall 子命令。
* --update-apply 选项变更为 update 子命令。
* 若要使用 --update-check，请使用包含 --check-only 选项的 update 子命令。

之前可用的其他选项仍可用于各自的子命令。 每个子命令的单独帮助可通过 -h 或 --help 选项获得：`dotnet new <subcommand> --help` 列出子命令的所有支持选项。

#### 2.1 dotnet new list
dotnet new list - 列出要使用 dotnet new 运行的可用模板。

```shell
dotnet new list [<TEMPLATE_NAME>] [--author <AUTHOR>] [-lang|--language {"C#"|"F#"|VB}]
    [--tag <TAG>] [--type <TYPE>] [--columns <COLUMNS>] [--columns-all]
    [-o|--output <output>] [--project <project>] [--ignore-constraints]
    [-d|--diagnostics] [--verbosity <LEVEL>] [-h|--help]
```