## [.NET CLI 、NuGet](#)
> .NET CLI（Command-Line Interface）是.NET的命令行工具，提供了创建、构建、运行和发布应用程序的功能，它是开发者在命令行环境中使用.NET进行开发和管理项目的主要工具。
> 详情请查看[.NET CLI 官方文档](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet)。
> 
> NuGet 是一个开放源代码的包管理器，主要用于 .NET 平台上的项目开发。它允许开发人员轻松地创建、共享和使用可重复使用的代码库（称为“包”）。
> 详情请查看[NuGet 官方文档](https://learn.microsoft.com/zh-cn/nuget/what-is-nuget)

### [.NET CLI 命令列表](#)
.NET 命令行接口 (CLI) 工具是用于开发、生成、运行和发布 .NET 应用程序的跨平台工具链。

[dotnet命令](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet) 主要有三种功能
* 获取有关可用命令和环境的信息
* 运行命令（需要 SDK 安装）
* 运行.NET应用程序
```shell
#获取有关可用命令和环境的信息：
dotnet [--version] [--info] [--list-runtimes] [--list-sdks]

dotnet -h|--help
#运行命令
dotnet build --configuration Release
#运行.NET应用程序
dotnet exec ./bin/Debug/net6.0/MyApp.dll arg1 arg2
```
**常用命令:**

| [dotnet](contents/dotnet_cli_description.md) | [dotnet new](contents/dotnet_cli_cmd_new.md) | dotnet build  | dotnet run     | dotnet store | dotnet clean   | dotnet help   |
|:---------------------|:-------------------------|:--------------|:---------------|:-------------|:---------------|:--------------|
| **dotnet test**      | **dotnet vstest**        | **dotnet pack** | **dotnet migrate** | **dotnet sln**   | **dotnet publish** | **dotnet exec** |
| **dotnet restore**   |                          |               |                |              |                |               |