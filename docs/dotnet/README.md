## [.NET 架构须知](#)
> **介绍**：了解.NET 发展历史以及现状，理解.NET Framework、CLR、.NETCore、CTS、JIT等概念。




### .NET 简介 
.NET 是由 Microsoft 支持的免费开放源代码应用程序平台。

.NET框架有多个实现,如.NET Framework、.NET Core（及后续的.NET 5+版本），以及社区版本Mono。
在2020年，微软将各个实现统一为.NET平台，使得具备完全的跨平台功能。

<img src="./static/netcore.png"  width="1100px"  />

#### 1.1 发展历史
历时20年，.NET已经前进到 [.NET 9.0](https://learn.microsoft.com/zh-cn/dotnet/core/whats-new/dotnet-9/overview)大版本。

<img src="./static/f38b711f-dd77-49fe-9eeb-f347241a2787.png"  width="1000px"  />

推荐一个在线的版本，做的非常漂亮：[Microsoft .NET History](https://time.graphics/embed?v=1&id=593132)

* .**NET Framework(1.0 —— 4.8.1)**：.NET Framework是基于Windows系统的.NET框架，从2002年发布，到最新的4.8.*版本，已经停止发展。
    * 最后的4.8.*版本依然还在维护，还是可以使用的，支持的最低操作系统是Windows 7。
    * 如果要运行在XP系统上，则只能使用.NET Framework4版本，支持最低Windows XP SP3。
    * .NET Framework是基于Windows系统的，因此也只能在Windows系统上运行。

* .**NET Core(Core1/2/3，5/6/7/8/9)**：从2016年发布首个.NET Core1，和后面的.NET Core2/3、.NET 5/6/7/*是一个体系的，只是从.NET5开始更改了命名。这是微软推出的新一代.NET框架，用来代替原有的.NET Framework，核心特点就是开源、跨平台，这也是.NET未来重点发展、投资的地方。

> 开源，采用MIT和Apache协议作为开源协议，对商业十分友好。  
> 跨平台，支持Windows、MacOS、Linux，支持x64,、x86、ARM架构。
