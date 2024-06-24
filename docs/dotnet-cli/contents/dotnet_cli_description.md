## [dotnet 命令详解](#)
**介绍**：dotnet - .NET CLI 的通用驱动程序，[官方文档。](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet#options-for-running-an-application)

----
### [1. dotnet 命令](#)
[dotnet命令](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet) 主要有三种功能
* 获取有关可用命令和环境的信息
* 运行命令
* 运行.NET应用程序

#### 1.1 获取有关可用命令和环境的信息
当自身使用 dotnet 时，可以使用以下选项，而无需指定要运行的命令或应用程序。 例如，dotnet --info 或 dotnet --version。 这些选项打印出有关环境的信息。

```shell
dotnet [--version] [--info] [--list-runtimes] [--list-sdks]

dotnet -h|--help
```
* **--info** : 打印出有关 .NET 安装和计算机环境（如当前操作系统）的详细信息，并提交 .NET 版本的 SHA。
* **--version** : 打印出 dotnet 命令使用的 .NET SDK 版本，该版本可能受 global.json 文件的影响。 只有安装了 SDK 后才可用。
* **--list-runtimes** : 打印出已安装的 .NET 运行时的列表。 x86 版本的 SDK 只列出 x86 运行时，而 x64 版本的 SDK 只列出 x64 运行时。
* **--list-sdks** : 打印出已安装的 .NET SDK 的列表。
* **-?|-h|--help** : 打印可用命令列表。

#### 1.2 运行命令
需要 SDK 安装, .NET SDK下载地址: [https://dotnet.microsoft.com/zh-cn/download](https://dotnet.microsoft.com/zh-cn/download)

以下选项适用于使用命令的 dotnet。 例如，dotnet build --help 或 dotnet build --verbosity diagnostic。
```shell
dotnet <command> [-d|--diagnostics] [-h|--help] [--verbosity <LEVEL>]
    [command-options] [arguments]
```
* **\<command\>** : 就是各种命令 build、new、clean、help等
* **-d|--diagnostics** : 启用诊断输出。
* **-v|--verbosity <LEVEL>** : 设置命令的详细级别。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 并非在每个命令中均受支持。 请参阅特定的命令页，确定此选项是否可用。
* **-?|-h|--help** : 打印出给定命令的文档。 例如，dotnet build --help 显示 build 命令的帮助。
* **command options** : 每个命令定义特定于该命令的选项。 有关可用选项的列表，请参阅特定命令页。

#### 1.3 运行应用程序
指定应用程序 .dll 文件的路径以运行应用程序。 运行应用程序即意味着找到并执行入口点，对于控制台应用，入口点是 Main 方法。
```shell
dotnet [--additionalprobingpath <PATH>] [--additional-deps <PATH>]
    [--fx-version <VERSION>]  [--roll-forward <SETTING>]
    <PATH_TO_APPLICATION> [arguments]

dotnet exec [--additionalprobingpath] [--additional-deps <PATH>]
    [--depsfile <PATH>]
    [--fx-version <VERSION>]  [--roll-forward <SETTING>]
    [--runtimeconfig <PATH>]
    <PATH_TO_APPLICATION> [arguments]
```


**用于运行应用程序的选项**
```shell
dotnet --roll-forward Major myapp.dll
```
* **-- additionalprobingpath \<PATH\>** : 包含要进行探测的探测策略和程序集的路径。 重复该选项以指定多个路径。
* **--additional-deps \<PATH\>** : 附加 .deps.json 文件的路径。 deps.json 文件包含依赖项、编译依赖项和用于解决程序集冲突的版本信息列表,[deps.json 详细信息](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md)。
* **--roll-forward \<SETTING\>** : 控制将前滚操作应用于应用的方式。 SETTING 可以为下列值之一。 如果未指定，则 Minor 为默认类型。
  * LatestPatch - 前滚到最高补丁版本。 这会禁用次要版本前滚。
  * Minor - 如果缺少所请求的次要版本，则前滚到最低的较高次要版本。 如果存在所请求的次要版本，则使用 LatestPatch 策略。
  * Major - 如果缺少所请求的主要版本，则前滚到最低的较高主要版本和最低的次要版本。 如果存在所请求的主要版本，则使用 Minor 策略。
  * LatestMinor - 即使存在所请求的次要版本，仍前滚到最高次要版本。 适用于组件托管方案。
  * LatestMajor - 即使存在所请求的主要版本，仍前滚到最高主要版本和最高次要版本。 适用于组件托管方案。
  * Disable - 不前滚。 仅绑定到指定的版本。 建议不要将此策略用于一般用途，因为它会禁用前滚到最新补丁的功能。 该值仅建议用于测试。
  * **开发环境** ： 使用默认的 LatestPatch 选项，确保你总是运行指定版本的最新修补程序，这样可以获得最新的 bug 修复和安全补丁。
  * **测试和部署** ：你可以使用 Minor 或 Major 选项来测试你的应用程序是否兼容更高的次要版本或主要版本。
  * **生产环境**： 可能需要使用 Disable 选项，以确保应用程序只运行在明确指定的版本上，避免任何潜在的不兼容性。
* **--fx-version \<VERSION\>** : **用于运行应用程序的 .NET 运行时版本**。此选项将重写应用程序 .runtimeconfig.json 文件中第一个框架引用的版本。 这意味着，仅当只有一个框架引用时，它才会按预期方式工作。 如果应用程序具有多个框架引用，则使用此选项可能会导致错误。

**使用 exec 命令运行应用程序的选项**
```shell
dotnet exec --runtimeconfig myapp.runtimeconfig.json myapp.dll
```
仅当 dotnet 使用 exec 命令运行应用程序时，以下选项才可用。
* **--depsfile \<PATH\>** deps.json 文件的路径。 `.deps.json` 文件是一个配置文件，其中包含有关运行应用程序所需的依赖项的信息。 此文件由 .NET SDK 生成。
* **--runtimeconfig \<PATH\>** runtimeconfig.template.json 文件的路径。 runtimeconfig.json 文件包含运行时设置，通常命名为 <applicationname>.runtimeconfig.json。 [有关详细信息请参阅 .NET 运行时配置设置](https://learn.microsoft.com/zh-cn/dotnet/core/runtime-config/#runtimeconfigjson)。

### 1.4 例子

查看是否安装 dotnet
```shell
#版本信息
dotnet --version
#基本信息 
dotnet --info
```
在当前目录创建一个 `C#` 控制台程序
```shell
dotnet new console -lang "C#" --use-program-main
```
然后我们看到代码
```csharp
namespace workshop;

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hello, World!");
    }
}
```
编译运行
```shell
dotnet build

dotnet run
```

