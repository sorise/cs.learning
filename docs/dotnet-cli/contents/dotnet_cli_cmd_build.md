## [dotnet build 命令](#)
**介绍**： 生成项目及其所有依赖项。 [官方文档](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-build)

-----

### [1. build 命令](#)
生成项目及其所有依赖项。
```shell
dotnet build [<PROJECT>|<SOLUTION>] [-a|--arch <ARCHITECTURE>]
    [--artifacts-path <ARTIFACTS_DIR>]
    [-c|--configuration <CONFIGURATION>] [-f|--framework <FRAMEWORK>]
    [--disable-build-servers]
    [--force] [--interactive] [--no-dependencies] [--no-incremental]
    [--no-restore] [--nologo] [--no-self-contained] [--os <OS>]
    [-o|--output <OUTPUT_DIRECTORY>]
    [-p|--property:<PROPERTYNAME>=<VALUE>]
    [-r|--runtime <RUNTIME_IDENTIFIER>]
    [--self-contained [true|false]] [--source <SOURCE>]
    [--tl:[auto|on|off]] [--use-current-runtime, --ucr [true|false]]
    [-v|--verbosity <LEVEL>] [--version-suffix <VERSION_SUFFIX>]

dotnet build -h|--help
```

* **PROJECT | SOLUTION** 要生成的项目或解决方案文件。 如果未指定项目或解决方案文件，MSBuild 会在当前工作目录中搜索文件扩展名以 proj 或 sln 结尾的文件并使用该文件。
* **-f|--framework \<FRAMEWORK\>** 编译特定框架。 必须在项目文件中定义该框架。 示例：net7.0、net462。
* **-a|--arch \<ARCHITECTURE\>** 指定目标体系结构。 这是用于设置[运行时标识符 (RID)](https://learn.microsoft.com/zh-cn/dotnet/core/rid-catalog) 的简写语法，其中提供的值与默认 RID 相结合。
  * win-x64 、win-x86、win-arm64
  * linux-x64 、 linux-arm64
  * osx-x64、osx-arm64
  * ios-arm64、android-arm64
* **-p|--property:\<PROPERTYNAME\>=\<VALUE\>** 设置一个或多个 MSBuild 属性。 指定以分号分隔的多个属性，或通过重复该选项指定多个属性：

### [2. 使用例子](#)
生成项目及其依赖项：
```shell
dotnet build
```