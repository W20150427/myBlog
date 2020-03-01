---
title: C#6.0语法
date: 2020-02-27
tags: [程序设计语言，C#]
categories: csharp程序设计语言
---
C#6.0语法（发布于 2015，.NET Framework 4.6 .NET Core 1.0 .NET Core 1.1）
C# 在 3.0 版和 5.0 版对面向对象的语言添加了主要的新功能。 版本 6.0 随 Visual Studio 2015 一起发布，通过该版本，它不再推出主导性的杀手锏，而是发布了很多使得 C# 编程更有效率的小功能。
<!-- more -->
参考：<https://docs.microsoft.com/zh-cn/dotnet/csharp/>
# <span style="color:#0366d6;">C#6.0语法</span>
## <span style="color:#0366d6;">只读自动属性</span>
>只读自动属性 提供了更简洁的语法来创建不可变类型
```csharp
public string FirstName { get; }
public string LastName { get;  }
```
>FirstName 和 LastName 属性只能在同一个类的构造函数的主体中设置
```csharp
public Student(string firstName, string lastName)
{
    if (IsNullOrWhiteSpace(lastName))
        throw new ArgumentException(message: "Cannot be blank", paramName: nameof(lastName));
    FirstName = firstName;
    LastName = lastName;
}
```
## <span style="color:#0366d6;">自动属性初始化表达式</span>
>自动属性初始值设定项 可让你在属性声明中声明自动属性的初始值
```csharp
public ICollection<double> Grades { get; } = new List<double>();
```
## <span style="color:#0366d6;">Expression-bodied 函数成员</span>
>你编写的许多成员是可以作为单个表达式的单个语句。 改为编写 expression-bodied 成员。 这适用于方法和只读属性。 例如，重写 ToString() 通常是理想之选
```csharp
public override string ToString() => $"{LastName}, {FirstName}";
```
>也可以将此语法用于只读属性：
```csharp
public string FullName => $"{FirstName} {LastName}";
```
## <span style="color:#0366d6;">using static</span>
>using static 增强功能可用于导入单个类的静态方法。 指定要使用的类
```csharp
using static System.Math;
```
>使用using static时，扩展方法不作为静态方法导入，只有普通的静态方法导入
```csharp
using static System.String;
```
## <span style="color:#0366d6;">Null 条件运算符</span>
>如果 Person 对象是 null，则将变量 first 赋值为 null。 否则，将 FirstName 属性的值分配给该变量。 最重要的是，?. 意味着当 person 变量为 null 时，此行代码不会生成 NullReferenceException。 它会短路并返回 null。
```csharp
var first = person?.FirstName;
```
>还可以将 ?. 用于有条件地调用方法。 具有 null 条件运算符的成员函数的最常见用法是用于安全地调用可能为 null 的委托（或事件处理程序）。 通过使用 ?. 运算符调用该委托的 Invoke 方法来访问成员。
```csharp
// preferred in C# 6:
this.SomethingHappened?.Invoke(this, eventArgs);
```
## <span style="color:#0366d6;">字符串内插</span>
>使用 $ 作为字符串的开头，并使用 { 和 } 之间的表达式代替序号：
```csharp
public string FullName => $"{FirstName} {LastName}";
```
## <span style="color:#0366d6;">异常筛选器</span>
>异常筛选器是确定何时应该应用给定的 catch 子句的子句 。 如果用于异常筛选器的表达式计算结果为 true，则 catch 子句将对异常执行正常处理。 如果表达式计算结果为 false，则将跳过 catch 子句。 一种用途是检查有关异常的信息，以确定 catch 子句是否可以处理该异常
```csharp
public static async Task<string> MakeRequest()
{
    WebRequestHandler webRequestHandler = new WebRequestHandler();
    webRequestHandler.AllowAutoRedirect = false;
    using (HttpClient client = new HttpClient(webRequestHandler))
    {
        var stringTask = client.GetStringAsync("https://docs.microsoft.com/en-us/dotnet/about/");
        try
        {
            var responseText = await stringTask;
            return responseText;
        }
        catch (System.Net.Http.HttpRequestException e) when (e.Message.Contains("301"))
        {
            return "Site Moved";
        }
    }
}
```
## <span style="color:#0366d6;">nameof 表达式</span>
>nameof 表达式的计算结果为符号的名称。 每当需要变量、属性或成员字段的名称时，这是让工具正常运行的好办法。 nameof 的其中一个最常见的用途是提供引起异常的符号的名称
```csharp
if (IsNullOrWhiteSpace(lastName))
    throw new ArgumentException(message: "Cannot be blank", paramName: nameof(lastName));
```
## <span style="color:#0366d6;">Catch 和 Finally 块中的 Await</span>
>C# 5 对于可放置 await 表达式的位置有若干限制。 使用 C# 6，现在可以在 catch 或 finally 表达式中使用 await。 这通常用于日志记录方案：
```csharp
public static async Task<string> MakeRequestAndLogFailures()
{ 
    await logMethodEntrance();
    var client = new System.Net.Http.HttpClient();
    var streamTask = client.GetStringAsync("https://localHost:10000");
    try {
        var responseText = await streamTask;
        return responseText;
    } catch (System.Net.Http.HttpRequestException e) when (e.Message.Contains("301"))
    {
        await logError("Recovered from redirect", e);
        return "Site Moved";
    }
    finally
    {
        await logMethodExit();
        client.Dispose();
    }
}
```
## <span style="color:#0366d6;">使用索引器初始化关联集合</span>

```csharp
private Dictionary<int, string> messages = new Dictionary<int, string>
{
    { 404, "Page not Found"},
    { 302, "Page moved, but left a forwarding address."},
    { 500, "The web server can't come out to play today."}
};
```
```csharp
private Dictionary<int, string> webErrors = new Dictionary<int, string>
{
    [404] = "Page not Found",
    [302] = "Page moved, but left a forwarding address.",
    [500] = "The web server can't come out to play today."
};
```
## <span style="color:#0366d6;">集合初始值设定项中的扩展 Add 方法</span>
>todo
## <span style="color:#0366d6;">改进了Task.Run重载解析</span>
```csharp
static Task DoThings() 
{
     return Task.FromResult(0); 
}
```
>早期版本的 C# 中，使用方法组语法调用该方法将失败
```csharp
Task.Run(DoThings); 
```
>早期的编译器无法正确区分 Task.Run(Action) 和 Task.Run(Func<Task>()),
早期版本中，需要使用 lambda 表达式作为参数,貌似对Action仍不支持（todo）
```csharp
Task.Run(() => DoThings());
```