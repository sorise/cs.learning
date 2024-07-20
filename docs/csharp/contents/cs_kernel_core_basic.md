## [C#启航、语言基础](#)
> **介绍** : 学习如何创建dotnet程序，C#语言的基本知识。
----

- [1. 程序入口Main方法](#1-程序入口main方法)


---
### [1. 程序入口Main方法](#)
C#程序从方法Main开始执行的，Main方法一般位于Program类中，根据执行环境有不同的要求。

1. 使用static修饰符 
2. 在任何名称的类中 
3. 返回int或者void类型。

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

### [2. 变量](#)
C#是一门纯面向对象的,允许使用struct和class创建值类型和引用类型。Java只允许创建自定义引用类型。

C++中struct和class之间的区别知识访问修饰符的默认值不同。 .NET的类在命名空间中组织。

变量声明语法
```c#
DataType Identifier;
--------------------------
//值类型
Int32 value = 32;
Int32 a = 12. b = 23, c = 0;

//声明一个类，引用类型
User user = new User("jxkicker", 26 , true);
```

**常量**: 变量前面加一个const 关键字修饰 就是一个常量了
* 要求: 声明的时候必须初始化,指定值后不能再次改变
* 要求: 不能从变量中提取的值来初始化常量
* 说明: 常量本身就是静态的 不需要static 修饰
* 说明: 在编译的时候就需要有确定的值，只能用于数值和字符串 不能用于引用类型

**类型推断var** ：类型推断使用 var 关键字,用来声明并初始化局部变量。编译器根据=右边的语句推断出变量实际的类型。
* 要求:变量一旦声明必须要初始化,初始化不能呢为null
* 要求:一旦推到就无法更改类型
* 要求:初始化器必须放到表达式中
```C#
var name="Tom Cheng";
Type nameType=name.GetType();
WriteLine($"Type: {nameType}");

/*
输出:Type: System.String
*/

var user = new DCL.User("Nmae", 15);
Type typeOfuser = user.GetType();
WriteLine(typeOfuser.FullName); //Grammar.DCL.User
```

### 2.1 变量的初始化问题
变量一旦声明就应该初始化，避免空指针异常 例如：
```C#
public class Person
{
    public static Int64 userCount = 0;
    
    private String _name;
    private Int16 _age;
    private float _score = 0;
    
    public String Name { get; set; }
    public Int16 Age { get; set; }
    public float Score
    {
        get => _score;
        set => _score = value > 60 ? value : 0;
    }
    private readonly Int32 _onlyKey;

    public Int32 OnlyKey { get => this._onlyKey > 0?this._onlyKey:-1; }
}

Person li;
String name = li.Name; 
//这样的错误经常犯,有时候代码相隔太远,或者是忘了判断 就会导致空指针异常
```
**建议** : 多使用空值传播运算符使得代码更加简单: `name=li?.Name`; 
如何变量是一个引用，就可以把其值设置为null，表示他不引用任何对象
```c#
Person user = null;
```

### [3. 值类型和引用类型](#)
C# 把数据类型分为值类型和引用类型, 值类型存储值，引用类型存储对值的引用。这两种类型存储在内存的不同地方，值类型存储在堆栈[stack]中，而引用类型存储在托管堆[managed heap]。

垃圾回收机制[gc]会删除不能访问不再使用的对象,进行垃圾回收释放占用内存,而他们最明显的不同可能要体验在对这两种类型的复制之上。

* **值类型的复制**: ageOfSinme 虽然是复制的ageOfTom 的值，但是他们完全是不一样的两个值，分别占据内存的两个位置。如同张三按照李四的房子建造了一个和张三一样的属于李四的房子。
* **引用类型的复制**: 类似于钥匙的复制，房子只有一个，实例化后 me 获得房子的访问钥匙，然后other复制了这把钥匙，但是房子只有一个。me 和 other 都拿着同一个房子的钥匙。