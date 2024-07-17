## [C#启航、语言基础](#)
> **介绍** : 学习如何创建dotnet程序，C#语言的基本知识。
----

- [1. 程序入口Main方法](#1-程序入口main方法)


---
### [1. 程序入口Main方法](#)
C#程序从方法Main开始执行的，Main方法一般位于Program类中，根据执行环境有不同的要求。

1. 使用static修饰符 
2. 在任何名称的类中 
3.返回int或者void类型。

在当前目录创建一个 C# 控制台程序
```shell
cd workshop #随意进入你想进入的目录中
dotnet new console -lang "C#" --use-program-main
```
然后我们看到代码
```c#
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

#### [1.1 c# 顶级语句](#)
**C# 9.0** 引入了一种新的编程模式，称为顶级语句（Top-level Statements），这是一种更简洁的方式来编写简单的程序。  
**顶级语句允许你在没有显式声明类和Main方法的情况下编写代码**，从而简化代码结构，使得编写脚本和小型程序更加方便。  
**语法**,代码等价于上面传统的C#程序：
```c#
// Using top-level statements
using System;

Console.WriteLine("Hello, World!");
```
**特性和限制**：
**一个文件中的顶级语句**：一个项目中只能有一个文件包含顶级语句。如果多个文件中包含顶级语句，编译器将会报错。  
**命名空间和引用**：你仍然可以使用using指令来引用命名空间和库，这些指令通常放在文件的顶部。  
**异步支持**：顶级语句支持异步编程，你可以直接使用await关键字。例如：  
```c#
using System;
using System.Net.Http;
using System.Threading.Tasks;

HttpClient client = new HttpClient();
var response = await client.GetStringAsync("https://api.test.com/auth");
Console.WriteLine(response);
```
**局部函数和方法**：你可以在顶级语句中定义局部函数和方法，这些函数和方法只能在顶级语句中使用。
```c#
using System;

Console.WriteLine("Hello, World!");

void SayHello(string name)
{
    Console.WriteLine($"Hello, {name}!");
}

SayHello("C#");
```
无论是进行简单的计算、发起HTTP请求，还是进行文件操作，顶级语句都可以使代码更加简洁和直观。

#### [1.2 顶级语句原理](#)
C#作为面向对象的编程语言，一切类型都是面向对象的，要有类型和成员定义。顶级语句表面看着好像违反了这一规则，实际上没有。这是因为，顶级语句最终还是在编译的时候，被作为全局命空间中Program类的Main方法体中一段代码一起自动生成。如下所示：

```c#
static class Program
{
    static async Task Main(string[] args)
    {
        // 顶级语句
    }
}
```
需要注意的是，这里的类名Program和方法名Main只是用来举例，其实在编译器生成的不是这个名字。

根据在顶级语句中是否有异步操作和返回值的情况，生成的入口函数签名也是不同的。具体如下面表格所示：

|	|存在返回值	|不存在返回值|
|:----|:----|:----|
|存在异步	|async static Task\<int\> Main(string[] args)|	async static Task Main(string[] args)|
|不存在异步 |	static int Main(string[] args)|	static void Main(string[] args)|

```c#
static class Program
{
    async static Task<int> Main(string[] args)
    {
        WriteLine("Hello");
        Print(args[0]);
        await Task.Delay(1000);
        return 0;
 
        void Print(string arg)
        {
            WriteLine(arg);
        }
    }
}
```

