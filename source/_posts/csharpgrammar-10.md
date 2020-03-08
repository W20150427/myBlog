---
title: C#8.0语法
date: 2020-02-29
tags: [程序设计语言，C#]
categories: csharp程序设计语言
---
C#8.0语法（发布于 2019，.NET Core 3.0）
<!-- more -->
参考：<https://docs.microsoft.com/zh-cn/dotnet/csharp/>
# <span style="color:#0366d6;">C#8.0语法</span>
## <span style="color:#0366d6;">内插逐字字符串的增强功能</span>
>内插逐字字符串中 $ 和 @ 标记的顺序可以任意安排：$@"..." 和 @$"..." 均为有效的内插逐字字符串。 在早期 C# 版本中，$ 标记必须出现在 @ 标记之前
## <span style="color:#0366d6;">Null 合并赋值</span>
>C# 8.0 引入了 null 合并赋值运算符 ??=。 仅当左操作数计算为 null 时，才能使用运算符 ??= 将其右操作数的值分配给左操作数
```csharp
List<int> numbers = null;
int? i = null;

numbers ??= new List<int>();
numbers.Add(i ??= 17);
numbers.Add(i ??= 20);

Console.WriteLine(string.Join(" ", numbers));  // output: 17 17
Console.WriteLine(i);  // output: 17
```
## <span style="color:#0366d6;">using 声明</span>
>using 声明 是前面带 using 关键字的变量声明。 它指示编译器声明的变量应在封闭范围的末尾进行处理
```csharp
static int WriteLinesToFile(IEnumerable<string> lines)
{
using var file = new System.IO.StreamWriter("WriteLines2.txt");
// Notice how we declare skippedLines after the using statement.
int skippedLines = 0;
foreach (string line in lines)
{
    if (!line.Contains("Second"))
    {
        file.WriteLine(line);
    }
    else
    {
        skippedLines++;
    }
}
// Notice how skippedLines is in scope here.
return skippedLines;
// file is disposed here
}
```
## <span style="color:#0366d6;">可处置的 ref 结构</span>
>用 ref 修饰符声明的 struct 可能无法实现任何接口，因此无法实现 IDisposable。 因此，要能够处理 ref struct，它必须有一个可访问的 void Dispose() 方法。 此功能同样适用于 readonly ref struct 声明。
## <span style="color:#0366d6;">静态本地函数</span>
>考虑下列代码。 本地函数 LocalFunction 访问在封闭范围（方法 M）中声明的变量 y。 因此，不能用 static 修饰符来声明 LocalFunction：
```csharp
int M()
{
int y;
LocalFunction();
return y;

void LocalFunction() => y = 0;
}
```
>下面的代码包含一个静态本地函数。 它可以是静态的，因为它不访问封闭范围中的任何变量
```csharp
int M()
{
int y = 5;
int x = 7;
return Add(x, y);

static int Add(int left, int right) => left + right;
}
```
## <span style="color:#0366d6;">Readonly 成员</span>
>在 readonly 成员定义中，readonly 表示 struct 的成员不会改变结构的内部状态
其他情况下，你可以创建支持可变的结构。 在这些情况下，多个实例成员可能不会修改结构的内部状态。 可以使用 readonly 修饰符声明那些实例成员。 编译器会强制执行你的意图。 如果该成员直接修改状态或访问未使用 readonly 修饰符声明的成员，则结果为编译时错误。 readonly 修饰符对 struct 成员有效，而对 class 或 interface 成员声明无效

>readonly 修饰符对 struct 的大多数成员有效，包括重写在 System.Object 中声明的方法的方法。 但存在一些限制：
不能声明 readonly 静态方法或属性。
无法声明 readonly 构造函数。

```csharp
public struct Point
{
public double X { get; set; }
public double Y { get; set; }
public readonly double Distance => Math.Sqrt(X * X + Y * Y);

public readonly override string ToString() =>
$"({X}, {Y}) is {Distance} from the origin";
}
```
## <span style="color:#0366d6;">索引和范围</span>
>让我们从索引规则开始。 请考虑数组 sequence。 0 索引与 sequence[0] 相同。 ^0 索引与 sequence[sequence.Length] 相同。 请注意，sequence[^0] 不会引发异常，就像 sequence[sequence.Length] 一样。 对于任何数字 n，索引 ^n 与 sequence.Length - n 相同。
```csharp
var words = new string[]
{
                // index from start    index from end
    "The",      // 0                   ^9
    "quick",    // 1                   ^8
    "brown",    // 2                   ^7
    "fox",      // 3                   ^6
    "jumped",   // 4                   ^5
    "over",     // 5                   ^4
    "the",      // 6                   ^3
    "lazy",     // 7                   ^2
    "dog"       // 8                   ^1
};        
```
>可以使用 ^1 索引检索最后一个词：
```csharp
Console.WriteLine($"The last word is {words[^1]}");
// writes "dog"
```
>以下代码创建了一个包含单词“quick”、“brown”和“fox”的子范围。 它包括 words[1] 到 words[3]。 元素 words[4] 不在该范围内。
```csharp
var quickBrownFox = words[1..4];
```
>以下代码使用“lazy”和“dog”创建一个子范围。 它包括 words[^2] 和 words[^1]。 末尾索引 words[^0] 不包括在内：
```csharp
var lazyDog = words[^2..^0];
```
>下面的示例为开始和/或结束创建了开放范围：
```csharp
var allWords = words[..]; // contains "The" through "dog".
var firstPhrase = words[..4]; // contains "The" through "fox"
var lastPhrase = words[6..]; // contains "the", "lazy" and "dog"
```

>此外可以将范围声明为变量：
```csharp
Range phrase = 1..4;
var text = words[phrase];
```
## <span style="color:#0366d6;">默认接口方法</span>
>默认接口实现使开发人员能够升级接口，同时仍允许任何实现器替代该实现。 库的用户可以接受默认实现作为非中断性变更
<details>
<summary>展开查看</summary>

>ICustomer
```csharp
public interface ICustomer
{
IEnumerable<IOrder> PreviousOrders { get; }
DateTime DateJoined { get; }
DateTime? LastOrder { get; }
string Name { get; }
IDictionary<DateTime, string> Reminders { get; }

public static void SetLoyaltyThresholds(TimeSpan ago, int minimumOrders, decimal percentageDiscount)
{
length = ago; 
orderCount = minimumOrders;
discountPercent = percentageDiscount;
}
private static TimeSpan length = new TimeSpan(365 * 2, 0, 0, 0); // two years
private static int orderCount = 10;
private static decimal discountPercent = 0.10m;

// <SnippetFinalVersion>
public decimal ComputeLoyaltyDiscount() => DefaultLoyaltyDiscount(this);
protected static decimal DefaultLoyaltyDiscount(ICustomer c)
{
DateTime start = DateTime.Now - length;

if ((c.DateJoined < start) && (c.PreviousOrders.Count() > orderCount))
{
return discountPercent;
}
return 0;
}
// </SnippetFinalVersion>
}
```
>IOrder
```csharp
public interface IOrder
{
    DateTime Purchased { get; }
    decimal Cost { get; }
}
```
>SampleCustomer
```csharp
public class SampleCustomer : ICustomer
{
public SampleCustomer(string name, DateTime dateJoined) => 
(Name, DateJoined) = (name, dateJoined);

private List<IOrder> allOrders = new List<IOrder>();

public IEnumerable<IOrder> PreviousOrders => allOrders;

public DateTime DateJoined { get; }

public DateTime? LastOrder { get; private set; }

public string Name { get; }

private Dictionary<DateTime, string> reminders = new Dictionary<DateTime, string>();
public IDictionary<DateTime, string> Reminders => reminders;

public void AddOrder(IOrder order)
{
if (order.Purchased > (LastOrder ?? DateTime.MinValue))
LastOrder = order.Purchased;
allOrders.Add(order);
}

// <SnippetOverrideAndExtend>
public decimal ComputeLoyaltyDiscount()
{
if (PreviousOrders.Any() == false)
return 0.50m;
else
return ICustomer.DefaultLoyaltyDiscount(this);
}
// </SnippetOverrideAndExtend>
}
```
>SampleOrder
```csharp
 public class SampleOrder : IOrder
{
    public SampleOrder(DateTime purchase, decimal cost) =>
        (Purchased, Cost) = (purchase, cost);

    public DateTime Purchased { get; }

    public decimal Cost { get; }
}
```
```csharp
 static void Main(string[] args)
{
// <SnippetTestDefaultImplementation>
SampleCustomer c = new SampleCustomer("customer one", new DateTime(2010, 5, 31))
{
Reminders =
{
{ new DateTime(2010, 08, 12), "childs's birthday" },
{ new DateTime(1012, 11, 15), "anniversary" }
}
};

SampleOrder o = new SampleOrder(new DateTime(2012, 6, 1), 5m);
c.AddOrder(o);

o = new SampleOrder(new DateTime(2103, 7, 4), 25m);
c.AddOrder(o);

// <SnippetHighlightCast>
// Check the discount:
ICustomer theCustomer = c;
Console.WriteLine($"Current discount: {theCustomer.ComputeLoyaltyDiscount()}");
// </SnippetHighlightCast>
// </SnippetTestDefaultImplementation>

// Add more orders to get the discount:
DateTime recurring = new DateTime(2013, 3, 15);
for(int i = 0; i < 15; i++)
{
o = new SampleOrder(recurring, 19.23m * i);
c.AddOrder(o);

recurring.AddMonths(2);
}

Console.WriteLine($"Data about {c.Name}");
Console.WriteLine($"Joined on {c.DateJoined}. Made {c.PreviousOrders.Count()} orders, the last on {c.LastOrder}");
Console.WriteLine("Reminders:");
foreach(var item in c.Reminders)
{
Console.WriteLine($"\t{item.Value} on {item.Key}");
}
foreach (IOrder order in c.PreviousOrders)
Console.WriteLine($"Order on {order.Purchased} for {order.Cost}");

Console.WriteLine($"Current discount: {theCustomer.ComputeLoyaltyDiscount()}");

// <SnippetSetLoyaltyThresholds>
ICustomer.SetLoyaltyThresholds(new TimeSpan(30, 0, 0, 0), 1, 0.25m);
Console.WriteLine($"Current discount: {theCustomer.ComputeLoyaltyDiscount()}");
// </SnippetSetLoyaltyThresholds>
}
```
</details>

## <span style="color:#0366d6;">更多位模式</span>
### switch
```csharp
public enum Rainbow
{
    Red,
    Orange,
    Yellow,
    Green,
    Blue,
    Indigo,
    Violet
}
```
```csharp
public static RGBColor FromRainbow(Rainbow colorBand) =>
colorBand switch
{
    Rainbow.Red    => new RGBColor(0xFF, 0x00, 0x00),
    Rainbow.Orange => new RGBColor(0xFF, 0x7F, 0x00),
    Rainbow.Yellow => new RGBColor(0xFF, 0xFF, 0x00),
    Rainbow.Green  => new RGBColor(0x00, 0xFF, 0x00),
    Rainbow.Blue   => new RGBColor(0x00, 0x00, 0xFF),
    Rainbow.Indigo => new RGBColor(0x4B, 0x00, 0x82),
    Rainbow.Violet => new RGBColor(0x94, 0x00, 0xD3),
    _              => throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand)),
};
```
>1.变量位于 switch 关键字之前。 不同的顺序使得在视觉上可以很轻松地区分 switch 表达式和 switch 语句。
2.将 case 和 : 元素替换为 =>。 它更简洁，更直观。
3.将 default 事例替换为 _ 弃元。
4.正文是表达式，不是语句。

>原来的格式：
```csharp
public static RGBColor FromRainbowClassic(Rainbow colorBand)
{
switch (colorBand)
{
case Rainbow.Red:
    return new RGBColor(0xFF, 0x00, 0x00);
case Rainbow.Orange:
    return new RGBColor(0xFF, 0x7F, 0x00);
case Rainbow.Yellow:
    return new RGBColor(0xFF, 0xFF, 0x00);
case Rainbow.Green:
    return new RGBColor(0x00, 0xFF, 0x00);
case Rainbow.Blue:
    return new RGBColor(0x00, 0x00, 0xFF);
case Rainbow.Indigo:
    return new RGBColor(0x4B, 0x00, 0x82);
case Rainbow.Violet:
    return new RGBColor(0x94, 0x00, 0xD3);
default:
    throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand));
};
}
```
### 属性模式
```csharp
public static decimal ComputeSalesTax(Address location, decimal salePrice) =>
    location switch
    {
        { State: "WA" } => salePrice * 0.06M,
        { State: "MN" } => salePrice * 0.75M,
        { State: "MI" } => salePrice * 0.05M,
        // other cases removed for brevity...
        _ => 0M
    };
```
### 元组模式
```csharp
public static string RockPaperScissors(string first, string second)
    => (first, second) switch
    {
        ("rock", "paper") => "rock is covered by paper. Paper wins.",
        ("rock", "scissors") => "rock breaks scissors. Rock wins.",
        ("paper", "rock") => "paper covers rock. Paper wins.",
        ("paper", "scissors") => "paper is cut by scissors. Scissors wins.",
        ("scissors", "rock") => "scissors is broken by rock. Rock wins.",
        ("scissors", "paper") => "scissors cuts paper. Scissors wins.",
        (_, _) => "tie"
    };
```
### 位置模式

<details>
<summary>展开查看</summary>

```csharp
public class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y) => (X, Y) = (x, y);

    public void Deconstruct(out int x, out int y) =>
        (x, y) = (X, Y);
}
```
```csharp
public enum Quadrant
{
    Unknown,
    Origin,
    One,
    Two,
    Three,
    Four,
    OnBorder
}
```
```csharp
static Quadrant GetQuadrant(Point point) => point switch
{
    (0, 0) => Quadrant.Origin,
    var (x, y) when x > 0 && y > 0 => Quadrant.One,
    var (x, y) when x < 0 && y > 0 => Quadrant.Two,
    var (x, y) when x < 0 && y < 0 => Quadrant.Three,
    var (x, y) when x > 0 && y < 0 => Quadrant.Four,
    var (_, _) => Quadrant.OnBorder,
    _ => Quadrant.Unknown
};
```
</details>

## <span style="color:#0366d6;">异步流</span>
>todo
