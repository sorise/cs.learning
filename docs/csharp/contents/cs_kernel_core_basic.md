## [C#启航、语言基础](#)
> **介绍** : 学习如何创建dotnet程序，C#语言的基本知识。
----

- [1. 程序入口Main方法](#1-程序入口main方法)
- [2. 变量](#2-变量)
- [3. 数据类型](#3-数据类型)


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

#### 2.1 变量的初始化问题
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

### [3. 数据类型](#)
C# 把数据类型分为值类型、指针类型和引用类型, 值类型存储值，引用类型存储对值的引用。这两种类型存储在内存的不同地方，值类型存储在堆栈[stack]中，而引用类型存储在托管堆[managed heap]。

垃圾回收机制[gc]会删除不能访问不再使用的对象,进行垃圾回收释放占用内存,而他们最明显的不同可能要体验在对这两种类型的复制之上。

* **值类型的复制**: ageOfSinme 虽然是复制的ageOfTom 的值，但是他们完全是不一样的两个值，分别占据内存的两个位置。如同张三按照李四的房子建造了一个和张三一样的属于李四的房子。
* **引用类型的复制**: 类似于钥匙的复制，房子只有一个，实例化后 me 获得房子的访问钥匙，然后other复制了这把钥匙，但是房子只有一个。me 和 other 都拿着同一个房子的钥匙。
* **指针类型**：类似于C++。

#### [3.1 值类型](#)
值类型变量可以直接分配给一个值。它们是从类 System.ValueType 中派生的。

> 值类型直接存储数据的实际值，它们是分配在栈上的。当值类型被赋值给另一个变量或传递给函数时，复制的是该值本身，而不是其引用。C#中的值类型有以下几种：


|关键字|类型|描述|	范围|	默认值|
|:---|:----|:---|:---|:---|
|bool|System.Boolean|布尔值|True 或 False|	False|
|byte|System.Byte| 8位无符号整数	|0 到 255|	0|
|char|System.Char |16 位 Unicode 字符|	U +0000 到 U +ffff|	''|
|decimal|System.Decimal	|128 位精确的十进制值，28-29 有效位数|(-7.9 x 10^28 到 7.9 x 10^28) / 10^(0到28)|	0.0M|
|double|System.Double|64 位双精度浮点型|	(+/-)5.0 x 10^-324 到 (+/-)1.7 x 10^308|	0.0D|
|float|System.Single|32 位单精度浮点型|	-3.4 x 10^38 到 + 3.4 x 10^38	|0.0F|
|int|System.Int32|32 位有符号整数类型|-2,147,483,648 到 2,147,483,647 |0|
|long|System.Int64|64 位有符号整数类型|-923,372,036,854,775,808 到 9,223,372,036,854,775,807|	0L|
|sbyte|System.SByte|8 位有符号整数类型|-128 到 127|0|
|short|System.Int16|16 位有符号整数类型|-32,768 到 32,767|0|
|uint|System.UInt32|32 位无符号整数类型|	0 到 4,294,967,295|0|
|ulong|System.UInt64|64 位无符号整数类型|	0 到 18,446,744,073,709,551,615|0|
|ushort|System.UInt16|16 位无符号整数类型|	0 到 65,535|	0|
|enum|System.Enum| |  | |

```c#
enum ErrorCode : ushort
{
    None = 0,
    Unknown = 1,
    ConnectionLost = 100,
    OutlierReading = 200
}
```

如需得到一个类型或一个变量在特定平台上的准确尺寸，可以使用 sizeof 方法。表达式 **sizeof(type)** 产生以字节为单位存储对象或类型的存储尺寸。下面举例获取任何机器上 int 类型的存储尺寸：

#### [3.2 引用类型](#)
引用类型存储的是对象的引用，而不是实际的数据。引用类型的变量在栈上存储指向堆中对象的内存地址。当引用类型被赋值给另一个变量或传递给函数时，复制的是对象的引用，而不是对象本身。
内置的引用类型有：**object**、**dynamic** 和 **string**。

|关键字|类型|描述|
|:---|:----|:---|
|string|System.String|字符串类型,用于表示一系列字符。|
|数据类型||int[]、char[] | 
|class|类类型| | 
|interface|接口类型| | 
|delegate|委托类型| 可以理解为C++中的函数指针 or std::function |

**object**: 对象（Object）类型 是 C# 通用类型系统（Common Type System - CTS）中所有数据类型的**终极基类**。Object 是 **System.Object** 类的别名。所以对象（Object）类型可以被分配任何其他类型（值类型、引用类型、预定义类型或用户自定义类型）的值。但是，在分配值之前，需要先进行类型转换。

当一个值类型转换为对象类型时，则被称为**装箱**；另一方面，当一个对象类型转换为值类型时，则被称为**拆箱**。
```c#
int val = 8;

object obj = val;    //整型数据转换为了对象类型（装箱）
object obj2 = 100;   // 这是装箱
int nval = (int)obj; //再拆箱
```
**动态（Dynamic）类型** : 您可以存储任何类型的值在动态数据类型变量中。这些变量的类型检查是在运行时发生的。

声明动态类型的语法：
```c#
dynamic <variable_name> = value;  
//例如：
dynamic d = 20;
```
动态类型与对象类型相似，但是对象类型变量的类型检查是在编译时发生的，而动态类型变量的类型检查是在运行时发生的。

**字符串（String）类型** ：字符串（String）类型 允许您给变量分配任何字符串值。字符串（String）类型是 System.String 类的别名。它是从对象（Object）类型派生的。字符串（String）类型的值可以通过两种形式进行分配：**引号** 和 **@引号**。

例如：
```c#
String str = "runoob.com";
```
一个 @引号字符串：
```c#
@"runoob.com";
```
C# string 字符串的前面可以加 @（称作”逐字字符串”）将转义字符（\）当作普通字符对待，比如：
```c#
string str = @"C:\Windows";
```
等价于：
```C#
string str = "C:\\Windows";
```
@ 字符串中可以任意换行，换行符及缩进空格都计算在字符串长度之内。
```C#
string str = @"<script type=""text/javascript"">
    <!--
    -->
</script>";
```


#### [3.3 指针类型(Pointer types)](#)
指针类型变量存储另一种类型的内存地址。C# 中的指针与 C 或 C++ 中的指针有相同的功能。

声明指针类型的语法：
```c#
type* identifier;

//例如：
char* cptr;
int* iptr;
```
**注意**： C#中的指针类型通常需要在unsafe上下文中使用，并且在编译时需要启用unsafe选项。