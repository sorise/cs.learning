## [C#启航、语言基础](#)
> **介绍** : C# 是一种跨平台的通用语言，可以让开发人员在编写高性能代码时提高工作效率，学习如何创建dotnet程序，C#语言的基本知识。
----

- [1. 程序的通用结构](#1-程序的通用结构)
- [2. 变量](#2-变量)
- [3. 数据类型](#3-数据类型)
- [4. 值类型](#4-值类型)
- [5. 引用类型](#5-引用类型)
- [6. 指针类型](#6-指针类型)
- [7. 关键字](#7-关键字)
- [8. 命名空间](#8-命名空间)
- [9. 注释](#9-注释)
- [10. 预处理器指令](#10-预处理器指令)


---
### [1. 程序的通用结构](#)
C#程序从方法Main开始执行的，Main方法一般位于Program类中，根据执行环境有不同的要求。

1. 使用 **static** 修饰符 
2. 在任何名称的类中 
3. 返回int或者 **void** 类型。

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
dotnet build  #编译
dotnet run    #运行
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

#### [4. 值类型](#)
值类型变量可以直接分配给一个值。它们是从类 System.ValueType 中派生的。

> 值类型直接存储数据的实际值，它们是分配在栈上的。当值类型被赋值给另一个变量或传递给函数时，复制的是该值本身，而不是其引用。C#中的值类型有以下几种：

|关键字|类型|描述|	范围|	默认值|
|:---|:----|:---|:---|:---|
|bool|System.Boolean|布尔值|True 或 False|	False|
|char|System.Char |16 位 Unicode 字符|	U +0000 到 U +ffff|	''|
|decimal|System.Decimal	|128 位精确的十进制值，28-29 有效位数|(-7.9 x 10^28 到 7.9 x 10^28) / 10^(0到28)|	0.0M|
|double|System.Double|64 位双精度浮点型|	(+/-)5.0 x 10^-324 到 (+/-)1.7 x 10^308|	0.0D|
|float|System.Single|32 位单精度浮点型|	-3.4 x 10^38 到 + 3.4 x 10^38	|0.0F|
|int|System.Int32|32 位有符号整数类型|-2,147,483,648 到 2,147,483,647 |0|
|long|System.Int64|64 位有符号整数类型|-923,372,036,854,775,808 到 9,223,372,036,854,775,807|	0L|
|byte|System.Byte| 8位无符号整数	|0 到 255|	0|
|sbyte|System.SByte|8 位有符号整数类型|-128 到 127|0|
|short|System.Int16|16 位有符号整数类型|-32,768 到 32,767|0|
|uint|System.UInt32|32 位无符号整数类型|	0 到 4,294,967,295|0|
|ulong|System.UInt64|64 位无符号整数类型|	0 到 18,446,744,073,709,551,615|0|
|ushort|System.UInt16|16 位无符号整数类型|	0 到 65,535|	0|
|enum|System.Enum| |  | |
|[struct](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/struct)| | 支持readonly struct、 [ref struct](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/builtin-types/ref-struct) |  | |
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

#### [4.1 整型](#)
C#有八种整数，u加上就是无符号 不加就是有符号整数， 一般都用int 偶尔用 long 和short，sbyte、byte
在网络编程、音频处理相当常用。

数字分隔符: `_` 不添加任何功能，知识有助于提高可读性
* `0x`: 表示十六进制
* `0b`: 表示二进制
```c#
long value = 0x123_457_89ab_cd12;
long tiktok = 0b0110_0001_1111_0110;
Int32 val = 32;
Console.WriteLine($"val 二进制：{Convert.ToString(val, 2)}");
Console.WriteLine($"val 八进制：{Convert.ToString(val, 8)}");
Console.WriteLine($"val 十六进制：{Convert.ToString(val, 16)}");

/*
val 二进制：100000
val 八进制：40
val 十六进制：20
*/
```

* `U`: 无符号整数字面量 后缀 `uint age = 3568U;`
* `L`: 长整型字面量 后缀 `long age = 356800012L;`
* `UL`: 无符号长整型字面量 后缀 `ulong age = 356800012UL;`

#### [4.2 浮点类型](#)
这数值是四舍五入估计的值并不精确,精确度取决于小数位数,不建议使用浮点数对数值相近的浮点数比较大小，因为不精确。
如何在代码中对某个非整数值硬编译，则编译器一般嘉定该变量是double，如果要指定为float们可以后面加上字符,`F` 或 `f`。
```c#
float f = 13.3F
```

#### [4.3 decimal类型](#)
精度更高的浮点数,用于财务计算的专用类型，128位高精度十进制浮点数。

备注:如果制定数字为decimal那么 要如此: decimal d=12.333M; 数字后面要加一个M或m

```c#
decimal money = 8500.12m;
```

#### [4.4 char类型](#)
在C#中，char 数据类型确实使用16位（2字节）来存储一个Unicode字符。Unicode标准定义了世界上大多数语言的字符编码，包括中文。保存单个字符,使用单引号,16位的 Unicode 字符，U +0000 到 U +ffff。

一个char变量可以用来存储单个的Unicode字符。对于大多数现代语言中的字符，包括简体和繁体中文，一个char是足够的。这是因为Unicode的BMP（基本多文种平面，Basic Multilingual Plane）包含了大约65,536个字符，正好可以用16位来表示。

但是，有一些特殊的或非常少见的汉字可能位于BMP之外的其他平面，这些字符需要使用代理对（surrogate pair）来表示。代理对是由两个连续的char组成的，第一个char被称为高位代理（high surrogate），第二个char被称为低位代理（low surrogate）。在C#中，你可以使用string类型来存储包含这些特殊汉字的文本，因为string实际上是一个char数组，可以处理代理对。

例如，创建一个包含中文字符的char变量：
```c#
char chineseChar = '你';  // 这里'你'是一个中文汉字
Console.WriteLine(chineseChar);
```
如果要处理包含超出BMP范围的汉字的字符串，你可能会用到以下代码：
```c#
string complexString = "𠮷";  // 这个字由两个char组成，即一个代理对
foreach (char c in complexString)
{
    Console.WriteLine(c);
}
```
在这个例子中，complexString中的“𠮷”字实际上会输出两个char值，这代表了它的高位代理和低位代理。如果你需要遍历这样的字符串并正确处理每一个字符，应该使用StringInfo类的GetTextElementEnumerator方法，它能正确地处理代理对：
```c#
string complexString = "𠮷";
TextElementEnumerator enumerator = StringInfo.GetTextElementEnumerator(complexString);
while (enumerator.MoveNext())
{
    Console.WriteLine($"Character: {enumerator.GetTextElement()}");
}
```

#### [4.5 转义字符 和 @符号](#)
在字符串前面加一个@就可以避免使用转义符 string con=@"\n 是这个意思?" 结果原样输出: `\n  是这个意思?`

|名称|说明|
|-|-|
|`\'`|单引号|
|`\n`|换行符|
|`\"`|双引号|
|`\0`|空|
|`\r`|回车|
|`\\`|反斜线|
|`\t`|水平制表符|
|`\v`|垂直制表符|

#### [4.6 枚举](#)
enums 枚举是值类型，数据直接存储在栈中，而不是使用引用和真实数据的隔离方式来存储。本质上是整数类型：
* 枚举可以确保变量制定合法的期望的值
* 使得代码更加清晰,允许用描述性的名称表示整数
* 枚举使得代码更加易于输入

```c#
/* enum 是和类平级的不要写在类里面 */
enum MachineState
{
    PowerOff = 0,
    Running = 5,
    Sleeping = 10,
    Hibernating = Sleeping + 5
}

MachineState stat = MachineState.PowerOff;
```
枚举 Enum 提供了一些静态方法
* public static Array GetValues(Type enumType);
* public static string[] GetNames(Type enumType);
* public static bool TryParse(Type enumType, string? value, bool ignoreCase, out object? result);
* public static bool TryParse<TEnum>(string? value, bool ignoreCase, out TEnum result) where TEnum : struct;
* public static string? GetName(Type enumType, object value);


```c#
foreach(String name in Enum.GetNames(typeof(MachineState)))
{
    Enum.TryParse<MachineState>(name, out MachineState value);
    Console.WriteLine($"{name}: {(int)value} ");
}
```
#### [4.7 结构类型](#)
结构类型（**“structure type”**或**“struct type”**）是一种可封装数据和相关功能的值类型 。 使用 struct 关键字定义结构类型：

```c#
public class SizeClass
{
    public int Width { get; set; }
    public int Length { get; set; }
}

public struct SizeStruct
{
    public int Width { get; set; }
    public int Length { get; set; }
}

SizeStruct sizeStruct = new SizeStruct{Length = 10, Width = 10};

Console.WriteLine("赋值前：width= {0},length= {1}", sizeStruct.Width, sizeStruct.Length);
 
var copyOfSizeStruct = sizeStruct;
copyOfSizeStruct.Length = 5;
copyOfSizeStruct.Width = 5;

Console.WriteLine("赋值后：width= {0},length= {1}", sizeStruct.Width, sizeStruct.Length);
Console.WriteLine("赋值 copyOfSizeStruct：width= {0},length= {1}", copyOfSizeStruct.Width, copyOfSizeStruct.Length);
```

[C#中struct和class的区别详解](https://www.cnblogs.com/jjg0519/p/10341010.html):
* struct是值类型，**创建一个struct类型的实例被分配在栈上**。
* class是引用类型，创建一个class类型实例被分配在托管堆上。但struct和class的区别远不止这么简单。
* 结构可定义构造函数，但不能定义析构函数。但是，您不能为结构定义无参构造函数。无参构造函数(默认)是自动定义的，且不能被改变。
* 与类不同，结构不能继承其他的结构或类,**没有继承关系**。
* **结构可实现一个或多个接口**。
* 结构成员不能指定为 abstract、virtual 或 protected。
* 当您使用 New 操作符创建一个结构对象时，会调用适当的构造函数来创建结构。与类不同，结构可以不使用 New 操作符即可被实例化。
```c#
struct Point
{
    public int X;
    public int Y;
}

Point p = new Point();  // 使用new操作符
Point q;                // 直接声明
q.X = 10;
q.Y = 20;
```
* 如果不使用 New 操作符，只有在所有的字段都被初始化之后，字段才被赋值，对象才被使用。
* **结构变量通常分配在栈上，这使得它们的创建和销毁速度更快。但是，如果将结构用作类的字段，且这个类是引用类型，那么结构将存储在堆上**。
* 结构默认情况下是可变的，这意味着你可以修改它们的字段。但是，如果结构定义为只读，那么它的字段将是不可变的。


结构提供了一种轻量级的数据类型，适用于表示简单的数据结构，具有较好的性能特性和值语义：

结构可带有方法、字段、索引、属性、运算符方法和事件，适用于表示轻量级数据的情况，如坐标、范围、日期、时间等。

### [5. 引用类型](#)
引用类型存储的是对象的引用，而不是实际的数据。引用类型的变量在栈上存储指向堆中对象的内存地址。当引用类型被赋值给另一个变量或传递给函数时，复制的是对象的引用，而不是对象本身。
内置的引用类型有：**object**、**dynamic** 和 **string**。

|关键字|类型|描述|
|:---|:----|:---|
|string|System.String|字符串类型,用于表示一系列字符。|
|数组类型||int[]、char[] | 
|class|类类型| | 
|interface|接口类型| | 
|delegate|委托类型| 可以理解为C++中的函数指针 or std::function |

**object**: 对象（Object）类型 是 C# 通用类型系统（Common Type System - CTS）中所有数据类型的**终极基类**。Object 是 **System.Object** 类的别名。所以对象（Object）类型可以被分配任何其他类型（值类型、引用类型、预定义类型或用户自定义类型）的值。但是，在分配值之前，需要先进行类型转换。

Object类中有许多一般用途的基本方法：`Equals()`,`GetHashCode()`,`GetType()` `ToString()`

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


### [6. 指针类型](#)
指针类型(Pointer types)变量存储另一种类型的内存地址。C# 中的指针与 C 或 C++ 中的指针有相同的功能。

声明指针类型的语法：
```c#
type* identifier;

//例如：
char* cptr;
int* iptr;
```
**注意**： C#中的指针类型通常需要在unsafe上下文中使用，并且在编译时需要启用unsafe选项。

### [7. 关键字](#)
关键字是 C# 编译器预定义的保留字。这些关键字不能用作标识符，但是，如果您想使用这些关键字作为标识符，可以在关键字前面加上 @ 字符作为前缀。

在 C# 中，有些关键字在代码的上下文中有特殊的意义，如 get 和 set，这些被称为上下文关键字（contextual keywords）。

下表列出了 C# 中的保留关键字（Reserved Keywords）和上下文关键字（Contextual Keywords）

<table>
<tbody>
<tr>
    <td colspan="7"><b>保留关键字</b></td>
</tr>
<tr>
    <td>abstract</td>
    <td>as</td>
    <td>base</td>
    <td>bool</td>
    <td>break</td>
    <td>byte</td>
    <td>case</td>
</tr>
<tr>
    <td>catch</td>
    <td>char</td>
    <td>checked</td>
    <td>class</td>
    <td>const</td>
    <td>continue</td>
    <td>decimal</td>
</tr>
<tr>
    <td>default</td>
    <td>delegate</td>
    <td>do</td>
    <td>double</td>
    <td>else</td>
    <td>enum</td>
    <td>event</td>
</tr>
<tr>
    <td>explicit</td>
    <td>extern</td>
    <td>false</td>
    <td>finally</td>
    <td>fixed</td>
    <td>float</td>
    <td>for</td>
</tr>
<tr>
    <td>foreach</td>
    <td>goto</td>
    <td>if</td>
    <td>implicit</td>
    <td>in</td>
    <td>in (generic<br> modifier)</td>
    <td>int</td>
</tr>
<tr>
    <td>interface</td>
    <td>internal</td>
    <td>is</td>
    <td>lock</td>
    <td>long</td>
    <td>namespace</td>
    <td>new</td>
</tr>
<tr>
<td>null</td>
<td>object</td>
<td>operator</td>
<td>out</td>
<td>out<br> (generic<br> modifier)</td>
<td>override</td>
<td>params</td>
</tr>
<tr>
<td>private</td>
<td>protected</td>
<td>public</td>
<td>readonly</td>
<td>ref</td>
<td>return</td>
<td>sbyte</td>
</tr>
<tr>
<td>sealed</td>
<td>short</td>
<td>sizeof</td>
<td>stackalloc</td>
<td>static</td>
<td>string</td>
<td>struct</td>
</tr>
<tr>
<td>switch</td>
<td>this</td>
<td>throw</td>
<td>true</td>
<td>try</td>
<td>typeof</td>
<td>uint</td>
</tr>
<tr>
<td>ulong</td>
<td>unchecked</td>
<td>unsafe</td>
<td>ushort</td>
<td>using</td>
<td>virtual</td>
<td>void</td>
</tr>
<tr>
<td>volatile</td>
<td>while</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td colspan="7"><b>上下文关键字</b></td>
</tr>
<tr>
<td>add</td>
<td>alias</td>
<td>ascending</td>
<td>descending</td>
<td>dynamic</td>
<td>from</td>
<td>get</td>
</tr>
<tr>
<td>global</td>
<td>group</td>
<td>into</td>
<td>join</td>
<td>let</td>
<td>orderby</td>
<td>partial<br>(type)</td>
</tr>
<tr>
<td>partial<br> (method)</td>
<td>remove</td>
<td>select</td>
<td>set</td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody></table>

### [8. 命名空间](#)
C# 程序由一个或多个文件组成。 每个文件均包含零个或多个命名空间。 一个命名空间包含类、结构、接口、枚举、委托等类型或其他命名空间。

#### [8.1 声明命名空间](#)
C# 10.0提供了，[文件范围的命名空间声明](https://learn.microsoft.com/zh-cn/dotnet/csharp/whats-new/csharp-10#file-scoped-namespace-declaration)，这个新语法为 namespace 声明节省了水平和垂直空间。

**老版本**： 了解即可，不推荐再使用。
```c#
// A skeleton of a C# program
using System;

namespace YourNamespace
{
    class YourClass
    {
    }

    struct YourStruct
    {
    }

    interface IYourInterface
    {
    }

    delegate int YourDelegate();

    enum YourEnum
    {
    }

    namespace YourNestedNamespace
    {
        struct YourStruct
        {
        }
    }
}
```

**新版本**：类似于 Java 的 package 
```c#
//声明命名空间
namespace ponder.entities;

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
```

#### [8.2 引入命名空间](#)
using语句 用作引用命名空间 还可以给命名空间指定别名。

```c#
using System;
using System.Globalization;
using ponder.entities;

using collect = System.Collections;
```

### [9. 注释](#)
C# 有三类注释方式：单行注释、多行注释、XML注释。

#### [9.1 简单注释](#)
常规的双斜杠： `//` 单行注释。多行注释 使用 `/**/`。


#### [9.2 XML注释](#)
C#引入了新的XML注释，即我们在某个函数前新起一行，输入 **///**，VS.Net会自动增加XML格式的注释，这里整理一下可用的XML注释，[官方文档](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/xmldoc/recommended-tags)。

```c#
/// <summary>
///管理层的工资
/// </summary>
/// <param name="money">工资</param>
/// <param name="fh">分红</param>
/// <returns></returns>
public double Calculation(double money)
{
    int fh = 10000;
    return money + fh;
}
```

XML注释分为一级注释（Primary Tags）和二级注释（Secondary Tags），前者可以单独存在，后者必须包含在一级注释内部。

**I 一级注释**
1. `<remarks>` 对类型进行描述，功能类似 `<summary>`，据说建议使用`<remarks>`;
2. `<summary>` 对共有类型的类、方法、属性或字段进行注释；
3. `<value>`主要用于属性的注释，表示属性的制的含义，可以配合`<summary>`使用；
4. `<param>`用于对方法的参数进行说明，格式：`<param name="param_name">value</param>`；
5. `<returns>`用于定义方法的返回值，对于一个方法，输入///后，会自动添加`<summary>`、`<param>`列表和`<returns>`；
6. `<exception>`定义可能抛出的异常，格式：`<exception cref="IDNotFoundException">`；
7. `<example>`用于给出如何使用某个方法、属性或者字段的使用方法；
8. `<permission>`涉及方法的访问许可；
9. `<seealso>`用于参考某个其它的东东:)，也可以通过cref设置属性；
10. `<include>`用于指示外部的XML注释；
 
**II 二级注释**
1. `<c>` or `<code>`主要用于加入代码段；
2. `<para>`的作用类似HTML中的 `<p>` 标记符，就是分段；
3. `<pararef>` 用于引用某个参数；
4. `<see>`的作用类似 `<seealso>`，可以指示其它的方法；
5. `<list>`用于生成一个列表；
 
另外，还可以自定义XML标签 

**注释换行**：
```c#
///<summary> 
/// 基类（第 1 行） 
///<para> 说明：（第 2 行）</para> 
///<para>　封装一些常用的成员（第 3 行）</para> 
///<para>　前面要用全角空格才能显示出空格来（第 4 行）</para> 
///</summary> 
public class MyBase 
{ 
    /// <summary> 
    /// 构造函数（第 1 行） 
    ///<para> 说明：（第 2 行）</para> 
    ///<para>　　初始化一些数据（第 3 行）</para> 
    /// </summary> 
    public MyBase() 
    { 
        // 
        //TODO: 在此处添加构造函数逻辑 
        // 
    } 
}
```

**XML属性**：
```xml
<param name="name">description</param>
```
name：方法参数的名称。

```xml
<paramref name="name"/>
```
name：要引用的参数的名称。 用双引号 (" ") 将名称引起来。
```xml
<exception cref="member">description</exception>
```
cref = `"member"`：对当前编译环境中出现的一个异常的引用。 

### [10. 预处理器指令](#)
尽管编译器没有单独的预处理器，但本节中所述指令的处理方式与有预处理器时一样。 可使用这些指令来帮助条件编译。

#### 10.1 define undef
**#define**: 使用 #define 来定义符号。 将符号用作传递给 #if 指令的表达式时，该表达式的计算结果为 true。

**#undef**: 允许你定义一个符号，这样一来，通过将该符号用作 #if 指令中的表达式，表达式将计算为 false。

```c#
#undef DEBUG  
using System;  
class MyClass   
{  
    static void Main()   
    {  
        #if DEBUG   
            Console.WriteLine("DEBUG is defined");  
        #else  
            Console.WriteLine("DEBUG is not defined");  
        #endif  
    }  
}  
```

#### 10.2 条件编译
使用四个预处理器指令来控制条件编译,与C和C++ 不同，不能将数字值分配给符号。 C# 中的 #if 语句是布尔值，且仅测试是否已定义该符号。
* **#if**：打开条件编译，其中仅在定义了指定的符号时才会编译代码。
* **#elif**：关闭前面的条件编译，并基于是否定义了指定的符号打开一个新的条件编译。
* **#else**：关闭前面的条件编译，如果没有定义前面指定的符号，打开一个新的条件编译。
* **#endif**：关闭前面的条件编译。

```c#
#define MYTEST

#if (DEBUG && !MYTEST)
        Console.WriteLine("DEBUG is defined");
#elif (!DEBUG && MYTEST)
        Console.WriteLine("MYTEST is defined");
#elif (DEBUG && MYTEST)
        Console.WriteLine("DEBUG and MYTEST are defined");
#else
        Console.WriteLine("DEBUG and MYTEST are not defined");
#endif

#if (NET6_0_OR_GREATER || NETSTANDARD2_0_OR_GREATER)
    Console.WriteLine("Using .NET 6+ or .NET Standard 2+ code.");
#else
    Console.WriteLine("Using older code that doesn't support the above .NET versions.");
#endif
```

#### 10.3 定义区域
可以使用以下两个预处理器指令来定义可在大纲中折叠的代码区域：

* **#region**：启动区域。
* **#endregion**：结束区域。

```c#
#region MyClass definition
public class MyClass
{
    static void Main()
    {
    }
}
#endregion
```

#### 10.4 错误和警告信息
使用以下指令指示编译器生成用户定义的编译器错误和警告，并控制行信息：

* **#error**：使用指定的消息生成编译器错误。
* **#warning**：使用指定的消息生成编译器警告。
* **#line**：更改用编译器消息输出的行号。

```c#
#error Deprecated code in this method.

#warning Deprecated code in this method.

class MainClass
{
    static void Main()
    {
// 将下一行的行号强制设为 200
#line 200 "Special"
        int i;
        int j;
// 指令将行号恢复至默认行号
#line default
        char c;
        float f;
#line hidden // numbering not affected
        string s;
        double d;
    }
}
```