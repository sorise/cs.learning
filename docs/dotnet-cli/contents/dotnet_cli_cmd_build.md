## [dotnet build 命令](#)
**介绍**： 生成项目及其所有依赖项。 [官方文档](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet-build)

-----

### [1. build 命令](#)

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