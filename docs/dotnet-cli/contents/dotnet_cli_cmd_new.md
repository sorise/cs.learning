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
dotnet new list - 列出要使用 dotnet new 运行的可用模板。 [详情查看官方网站](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new-list)

```shell
dotnet new list [<TEMPLATE_NAME>] [--author <AUTHOR>] [-lang|--language {"C#"|"F#"|VB}]
    [--tag <TAG>] [--type <TYPE>] [--columns <COLUMNS>] [--columns-all]
    [-o|--output <output>] [--project <project>] [--ignore-constraints]
    [-d|--diagnostics] [--verbosity <LEVEL>] [-h|--help]
```
* **--author \<AUTHOR\>** 基于模板作者筛选模板。 支持部分匹配。 自 .NET SDK 5.0.300 之后可用。
* **--columns-all** 在输出中显示所有列。 自 .NET SDK 5.0.300 之后可用。
* **--ignore-constraints** 禁用检查模板是否满足要运行的约束。 自 .NET SDK 7.0.100 之后可用。
* **--columns \<COLUMNS\>** 要在输出中显示的列的以逗号分隔的列表。 支持的列包括：
  * language - 模板支持的语言的以逗号分隔的列表。
  * tags - 模板标记列表。
  * author - 模板作者。
  * type - 模板类型：项目或项。
* **-lang|--language {C#|F#|VB}** 根据模板支持的语言筛选模板。 接受的语言因模板而异。 对于某些模板无效。
* **--project \<PROJECT_PATH\>** 模板添加到的项目。 对于 list 命令，可能需要指定模板要添加到的项目，以便正确评估模板的约束。 自 .NET SDK 7.0.100 之后可用。
* **--tag \<TAG\>** 基于模板标记筛选模板。 若要选择，模板必须至少具有一个与条件完全匹配的标记。 自 .NET SDK 5.0.300 之后可用。
* **--type \<TYPE\>** 基于模板类型筛选模板。 预定义的值为 project、item 和 solution。

列出所有模板
```shell
dotnet new list
```
列出单页应用程序 (SPA) 模板：

```shell
dotnet new list spa
```
列出与“we”子字符串匹配的所有模板。
```shell
dotnet new list we
```

列出与支持 F# 语言的“we”子字符串匹配的所有模板。

```shell
dotnet new list we --language "F#"
```
列出所有项模板。
```shell
dotnet new list --type item
```

列出所有 C# 模板，从而在输出中显示作者和类型。
```shell
dotnet new list --language "C#" --columns "author,type"
```

#### 2.2 dotnet new search
dotnet new search - 在 NuGet.org 上搜索 dotnet new 支持的模板。 [详情查看官方网站](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-new-search)

```shell
dotnet new search <TEMPLATE_NAME>

dotnet new search [<TEMPLATE_NAME>] [--author <AUTHOR>] [-lang|--language <language>]
    [--package <PACKAGE>] [--tag <TAG>] [--type <TYPE>]
    [--columns <author|language|tags|type>] [--columns-all]
    [-d|--diagnostics] [--verbosity <LEVEL>] [-h|--help]
```
* **--author \<AUTHOR\>** 基于模板作者筛选模板。 支持部分匹配。 自 .NET SDK 5.0.300 之后可用。
* **--columns-all** 在输出中显示所有列。 自 .NET SDK 5.0.300 之后可用。
* **--ignore-constraints** 禁用检查模板是否满足要运行的约束。 自 .NET SDK 7.0.100 之后可用。
* **--columns \<COLUMNS\>** 要在输出中显示的列的以逗号分隔的列表。 支持的列包括：
* **--package \<PACKAGE\>** 基于 NuGet 包 ID 筛选模板。 支持部分匹配。
* **-lang|--language \<language\>** 根据模板支持的语言筛选模板。 接受的语言因模板而异，可能的语言为 C#、F#、VB、SQL、JSON、TypeScript 等。
* **--tag \<TAG\>** 基于模板标记筛选模板。 若要选择，模板必须至少具有一个与条件完全匹配的标记。 自 .NET SDK 5.0.300 之后可用。
* **--type \<TYPE\>** 基于模板类型筛选模板。 预定义的值为 project、item 和 solution。

#### 2.3 dotnet new install
dotnet new install 命令用于从提供的 PATH 或 NUGET_ID 安装模板包。 若要安装模板包的特定版本或预发布版本，请以 <package-name>::<package-version> 格式指定该版本。 

> 默认情况下，dotnet new 为该版本传递 `*`，它表示最新的稳定包版本。

```shell
dotnet new install <PATH|NUGET_ID>  [--interactive] [--add-source|--nuget-source <SOURCE>] [--force] 
    [-d|--diagnostics] [--verbosity <LEVEL>] [-h|--help]
```

* **\<PATH|NUGET_ID\>** 文件系统上的文件夹或要从中安装模板包的 NuGet 包标识符。 
 dotnet new 尝试从可用于当前工作目录的 NuGet 源以及通过 --add-source 选项指定的源安装 NuGet 包。 
 若要从 NuGet 源安装模板包的特定版本或预发布版本，请以 <package-name>::<package-version> 格式指定该版本。
* **--add-source|--nuget-source \<SOURCE\>** 默认情况下，`dotnet new install` 使用当前目录中的 NuGet 配置文件的层次结构来确定可从中安装包的 NuGet 源。
 如果指定了 --nuget-source，则会将源添加到要检查的源的列表中。 若要检查当前目录的配置源，请使用 dotnet nuget list source。 有关详细信息，请参阅常见的 NuGet 配置
* **-d|--diagnostics** 启用诊断输出。 自 .NET SDK 7.0.100 之后可用。
* **--force** 允许从指定的源安装模板包，即使它们将替代另一个源中的模板包。 自 .NET SDK 7.0.100 之后可用。
* **-h|--help** 打印 install 命令帮助。 自 .NET SDK 7.0.100 之后可用。
* **--interactive** 允许命令停止并等待用户输入或操作。 例如，完成身份验证。 自 .NET 5.0 SDK 起可用。
* **-v|--verbosity \<LEVEL\>** 设置命令的详细级别。 允许的值为 `q[uiet]、m[inimal]、n[ormal] 和 diag[nostic]`。 自 .NET SDK 7.0.100 之后可用。


使用交互模式从自定义 NuGet 源安装 ASP.NET Core 的 SPA 模板 2.0 版：
```shell
dotnet new 
--install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0 
--add-source "https://api.my-custom-nuget.com/v3/index.json" 
--interactive
```
#### 2.4 dotnet new uninstall
dotnet new uninstall 命令在提供的 PATH 或 NUGET_ID 中卸载模板包。

```shell
dotnet new uninstall <PATH|NUGET_ID> 
    [-d|--diagnostics] [--verbosity <LEVEL>] [-h|--help]
```

列出已安装的模板及其详细信息，包括如何卸载它们：
```shell
dotnet new uninstall
```

卸载 ASP.NET Core 的 SPA 模板：
```shell
dotnet new uninstall Microsoft.DotNet.Web.Spa.ProjectTemplates
```

#### 2.5 dotnet new update
更新已安装的模板包。

```shell
dotnet new update [--interactive] [--add-source|--nuget-source <SOURCE>] 
    [-d|--diagnostics] [--verbosity <LEVEL>] [-h|--help]

dotnet new update --check-only|--dry-run [--interactive] [--add-source|--nuget-source <SOURCE>] 
    [-d|--diagnostics] [--verbosity <LEVEL>] [-h|--help]
```

* **--check-only|--dry-run** 仅检查更新并显示要更新的模板包，而不应用任何更新。
* **--interactive** 允许命令停止并等待用户输入或操作。
* **--add-source|nuget-source \<SOURCE\>** 默认情况下，dotnet new install 使用当前目录中的 NuGet 配置文件的层次结构来确定可从中安装包的 NuGet 源。 
如果指定了 `--nuget-source`，则会将源添加到要检查的源的列表中。


更新已安装的模板包，同时使用交互模式检查自定义 NuGet 源：
```shell
dotnet new update --add-source "https://api.my-custom-nuget.com/v3/index.json" --interactive
```