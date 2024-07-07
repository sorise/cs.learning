## [.NET 架构须知](#)
> **介绍**：了解.NET 发展历史以及现状，理解.NET Framework、CLR、.NETCore、CTS、JIT等概念。




### .NET 简介 
.NET 是由 Microsoft 支持的免费开放源代码应用程序平台。

.NET框架有多个实现,如.NET Framework、.NET Core（及后续的.NET 5+版本），以及社区版本Mono。
在2020年，微软将各个实现统一为.NET平台，使得具备完全的跨平台功能。

<img src="./static/netcore.png"  width="1100px"  />

#### 1.1 发展历史
历时20年，.NET已经前进到 [.NET 9.0](https://learn.microsoft.com/zh-cn/dotnet/core/whats-new/dotnet-9/overview)大版本。

| 时间线 | .NET 版本 |语言特性|应用程序| Visual Studio | C# 版本 |
|--------|------------|------------|------------|----------------|---------|
| 2002   | .NET Framework 1.0, `only wins`|OOP| WebForm、WinForm| VS2002        | 1       |
| 2005   | .NET Framework 2.0, `only wins`|`泛型<T>`，迭代器、协变| ASP.NET |VS2005   | 2|
| 2007   | .NET Framework 3.*, `only wins` | Linq、Lambda | WPF、WCF、WF、EF| VS2008        | 3       |
| 2010   | .NET Framework 4.0, `only wins` | 动态语言、TPL(并行) | ASP.NET MVC |Windows XP SP3 | 4       |
| 2012   | .NET Framework 4.5, `only wins` | async、await | |Windows 7     | 5       |
| 2013   | .NET Framework 4.5.*| | | VS2013        | 6, nameof |
| 2015   | .NET Framework 4.6 |  | | VS2015        | 6, nameof |
| 2016   | .NET Core 1 `基于mono` `跨平台` |  `部分移植` | | -              | -       |
| 2017   | .NET Framework 4.7 | | | VS2017        | 7, out   |
| 2019   | .NET Core 2|  | ASP.NET Core、EF Core| -              | 8, readonly |
| 2019   | .NET Framework 4.8 | | | VS2019        | 7.3     |
| 2019   | .NET Core 3 | | blazor、WPF、WinForm、UWP、Json| | -              | 8, readonly |
| 2020   | .NET 5  `统一名称` | 支持调用JAVA |   | -              | 9, record |
| 2021   | .NET 6   | |  MAUI 发布 | VS2022         | 10, global using |
| 2022   | .NET 7   | |  MAUI 完善 | VS2022         | 11      |
| 2023   | .NET 8   | |   | VS2022         | 12      |
| 2024   | .NET 9   | `preview` |     | VS2022         | 12      |