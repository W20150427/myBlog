---
title: C#7.0语法
date: 2020-02-28
tags: [程序设计语言，C#]
categories: csharp程序设计语言
---
C#7.0语法  
7.0-2017-05-.NET Framework 4.7
7.1-2017-08-.NET Core 2.0
7.2-2017-10
7.3-2018-03-.NET Framework 4.8 .NET Core 2.1 .NET Core 2.2
<!-- more -->
参考：<https://docs.microsoft.com/zh-cn/dotnet/csharp/>
# <span style="color:#0366d6;">C#7.0语法</span>
## <span style="color:#0366d6;">out 变量</span>
>支持 out 参数的现有语法已在此版本中得到改进。 现在可以在方法调用的参数列表中声明 out 变量，而不是编写单独的声明语句
```csharp
if (int.TryParse(input, out int result))
    Console.WriteLine(result);
else
    Console.WriteLine("Could not parse input");
```
>隐式类型的局部变量
```csharp
if (int.TryParse(input, out var answer))
    Console.WriteLine(answer);
else
    Console.WriteLine("Could not parse input");
```
## <span style="color:#0366d6;">更多的 expression-bodied 成员</span>
>C# 6 为成员函数和只读属性引入了 expression-bodied 成员。 C# 7.0 扩展了可作为表达式实现的允许的成员。 在 C# 7.0 中，你可以在属性 和索引器 上实现构造函数 、终结器 以及 get 和 set 访问器。


```csharp
// Expression-bodied constructor
public ExpressionMembersExample(string label) => this.Label = label;

// Expression-bodied finalizer
~ExpressionMembersExample() => Console.Error.WriteLine("Finalized!");

private string label;

// Expression-bodied get / set accessors.
public string Label
{
    get => label;
    set => this.label = value ?? "Default label";
}
```
## <span style="color:#0366d6;">本地函数</span>
>本地函数使你能够在另一个方法的上下文内声明方法 。 本地函数使得类的阅读者更容易看到本地方法仅从声明它的上下文中调用。
```csharp
public static IEnumerable<char> AlphabetSubset3(char start, char end)
{
    if (start < 'a' || start > 'z')
        throw new ArgumentOutOfRangeException(paramName: nameof(start), message: "start must be a letter");
    if (end < 'a' || end > 'z')
        throw new ArgumentOutOfRangeException(paramName: nameof(end), message: "end must be a letter");

    if (end <= start)
        throw new ArgumentException($"{nameof(end)} must be greater than {nameof(start)}");

    return alphabetSubsetImplementation();

    IEnumerable<char> alphabetSubsetImplementation()
    {
        for (var c = start; c < end; c++)
            yield return c;
    }
}
```
>可以对 async 方法采用相同的技术，以确保在异步工作开始之前引发由参数验证引起的异常
```csharp
public Task<string> PerformLongRunningWork(string address, int index, string name)
{
    if (string.IsNullOrWhiteSpace(address))
        throw new ArgumentException(message: "An address is required", paramName: nameof(address));
    if (index < 0)
        throw new ArgumentOutOfRangeException(paramName: nameof(index), message: "The index must be non-negative");
    if (string.IsNullOrWhiteSpace(name))
        throw new ArgumentException(message: "You must supply a name", paramName: nameof(name));

    return longRunningWorkImplementation();

    async Task<string> longRunningWorkImplementation()
    {
        var interimResult = await FirstWork(address);
        var secondResult = await SecondStep(index, name);
        return $"The results are {interimResult} and {secondResult}. Enjoy.";
    }
}
```
## <span style="color:#0366d6;">throw表达式</span>
>从 C# 7.0 开始，throw 可以用作表达式和语句。 这允许在以前不支持的上下文中引发异常
### 条件运算符
```csharp
private static void DisplayFirstNumber(string[] args)
{
   string arg = args.Length >= 1 ? args[0] : 
                              throw new ArgumentException("You must supply an argument");
   if (Int64.TryParse(arg, out var number))
      Console.WriteLine($"You entered {number:F0}");
   else
      Console.WriteLine($"{arg} is not a number.");                            
}
```

### null 合并运算符
```csharp
public string Name
{
    get => name;
    set => name = value ?? 
        throw new ArgumentNullException(paramName: nameof(value), message: "Name cannot be null");
}  
```
### expression-bodied lambda 或方法
```csharp
DateTime ToDateTime(IFormatProvider provider) => 
         throw new InvalidCastException("Conversion to a DateTime is not supported.");
```
## <span style="color:#0366d6;">数字文本语法改进</span>
>C# 7.0 包括两项新功能，可用于以最可读的方式写入数字来用于预期用途：二进制文本和数字分隔符
```csharp
public const int Sixteen =   0b0001_0000;
public const int ThirtyTwo = 0b0010_0000;
public const int SixtyFour = 0b0100_0000;
public const int OneHundredTwentyEight = 0b1000_0000;
```
>对于十进制数字，通常将其用作千位分隔符：
```csharp
public const long BillionsAndBillions = 100_000_000_000;
```
>数字分隔符也可以与 decimal、float 和 double 类型一起使用
```csharp
public const double AvogadroConstant = 6.022_140_857_747_474e23;
public const decimal GoldenRatio = 1.618_033_988_749_894_848_204_586_834_365_638_117_720_309_179M;
```
## <span style="color:#0366d6;">元组</span>
>命名元组的新语言和库支持简化了设计工作：与类和结构一样，使用数据结构存储多个元素，但不定义行为。元组是包含多个字段以表示数据成员的轻量级数据结构,C# 7以前都是未命名的，只能通过 Item1 和 Item2 等引用，C# 7以后可以命名元祖的名字
### 命名元组和未命名元组
```csharp
var unnamed = ("one", "two");
```

```csharp
var named = (first: "one", second: "two");```

### 赋值和元组
>有相同元素数量的元组类型之间赋值，其中每个右侧元素都可被隐式转换为相应的左侧元素
```csharp
// The 'arity' and 'shape' of all these tuples are compatible. 
// The only difference is the field names being used.
var unnamed = (42, "The meaning of life");
var anonymous = (16, "a perfect square");
var named = (Answer: 42, Message: "The meaning of life");
var differentNamed = (SecretConstant: 42, Label: "The meaning of life");
```
>前两个变量（unnamed 和 anonymous）没有为元素提供语义名称。 字段名称为 Item1 和 Item2。 后两个变量（named 和 differentName）为元素提供了语义名称。 这两个元组具有不同的元素名称
```csharp
unnamed = named;

named = unnamed;
// 'named' still has fields that can be referred to
// as 'answer', and 'message':
Console.WriteLine($"{named.Answer}, {named.Message}");

// unnamed to unnamed:
anonymous = unnamed;

// named tuples.
named = differentNamed;
// The field names are not assigned. 'named' still has 
// fields that can be referred to as 'answer' and 'message':
Console.WriteLine($"{named.Answer}, {named.Message}");

// With implicit conversions:
// int can be implicitly converted to long
(long, string) conversion = named;
```
### 作为方法返回值的元组
>元组最常见的用途之一是作为方法返回值
```csharp
private static (double, double, int) ComputeSumAndSumOfSquares(IEnumerable<double> sequence)
{
    double sum = 0;
    double sumOfSquares = 0;
    int count = 0;

    foreach (var item in sequence)
    {
        count++;
        sum += item;
        sumOfSquares += item * item;
    }

    return (sum, sumOfSquares, count);
}
```
>建议为从方法返回的元组的元素提供语义名称
```csharp
private static (int Count, double Sum, double SumOfSquares) ComputeSumAndSumOfSquares(IEnumerable<double> sequence)
{
   //...
}
         
```
### 析构
>通过对方法返回的元组进行析构，可以解封元组中的所有项
```csharp
(int count, double sum, double sumOfSquares) = ComputeSumAndSumOfSquares(sequence);

```
>在括号外使用 var 关键字，隐式声明元组中每个字段的类型化变量
```csharp
var (sum, sumOfSquares, count) = ComputeSumAndSumOfSquares(sequence);
```
>在括号内将 var 关键字与任意或全部变量声明结合使用
```csharp
(double sum, var sumOfSquares, var count) = ComputeSumAndSumOfSquares(sequence);
```
### 析构用户定义类型
>定义一个或多个赋值给任意数量的 out 变量的 Deconstruct 方法
```csharp
public class Person
{
    public string FirstName { get; }
    public string LastName { get; }

    public Person(string first, string last)
    {
        FirstName = first;
        LastName = last;
    }

    public void Deconstruct(out string firstName, out string lastName)
    {
        firstName = FirstName;
        lastName = LastName;
    }
}
```
>该析构方法支持从 Person 赋值给两个表示 FirstName 和 LastName 属性的字符串
```csharp
var p = new Person("Althea", "Goodwin");
var (first, last) = p;
```
>Deconstruct 方法可以是一种扩展方法，用于解封对象的可访问数据成员。
```csharp
public class Student : Person
{
    public double GPA { get; }
    public Student(string first, string last, double gpa) :
        base(first, last)
    {
        GPA = gpa;
    }
}

public static class Extensions
{
    public static void Deconstruct(this Student s, out string first, out string last, out double gpa)
    {
        first = s.FirstName;
        last = s.LastName;
        gpa = s.GPA;
    }
}
```
>析构运算符不参与测试相等。 下面的示例生成编译器错误 CS0019
```csharp
Person p = new Person("Althea", "Goodwin");
if (("Althea", "Goodwin") == p)
    Console.WriteLine(p);
```
### 元组作为 out 参数
>元组自身可用作 out 参数 。不要与前面提到的析构函数部分中的任何多义性混淆。在方法调用中，只需描述元组的形状：
```csharp
Dictionary<int, (int, string)> dict = new Dictionary<int, (int, string)>();
dict.Add(1, (234, "First!"));
dict.Add(2, (345, "Second"));
dict.Add(3, (456, "Last"));

// TryGetValue already demonstrates using out parameters
dict.TryGetValue(2, out (int num, string place) pair);

Console.WriteLine($"{pair.num}: {pair.place}");

/*
 * Output:
 * 345: Second
 */
```
>还可以使用 unnamed 元组，并将其字段作为 Item1 和 Item2 引用
```csharp
dict.TryGetValue(2, out (int, string) pair);
// ...
Console.WriteLine($"{pair.Item1}: {pair.Item2}");
```
## <span style="color:#0366d6;">弃元</span>
>是一种在应用程序代码中人为取消使用的临时虚拟变量。 弃元相当于未赋值的变量；
通过将下划线 (_) 赋给一个变量作为其变量名，指示该变量为一个占位符变量。
```csharp
(_, _, area) = city.GetCityInformation(cityName);
```
### 元组和对象析构
<details>
<summary>展开查看 </summary>

```csharp
using System;
using System.Collections.Generic;

public class Example
{
    public static void Main()
    {
        var (_, _, _, pop1, _, pop2) = QueryCityDataForYears("New York City", 1960, 2010);

        Console.WriteLine($"Population change, 1960 to 2010: {pop2 - pop1:N0}");
    }
   
    private static (string, double, int, int, int, int) QueryCityDataForYears(string name, int year1, int year2)
    {
        int population1 = 0, population2 = 0;
        double area = 0;
      
        if (name == "New York City")
        {
            area = 468.48; 
            if (year1 == 1960)
            {
                population1 = 7781984;
            }
            if (year2 == 2010)
            {
                population2 = 8175133;
            }
            return (name, area, year1, population1, year2, population2);
        }

        return ("", 0, 0, 0, 0, 0);
    }
}
// The example displays the following output:
//     
```
</details>

>类、结构或接口的 Deconstruct 方法还允许从对象中检索和析构一组特定的数据。 如果想只使用析构值的一个子集时，可使用弃元。
<details>
<summary>展开查看 </summary>

```csharp
using System;

public class Person
{
    public string FirstName { get; set; }
    public string MiddleName { get; set; }
    public string LastName { get; set; }
    public string City { get; set; }
    public string State { get; set; }

    public Person(string fname, string mname, string lname, 
                  string cityName, string stateName)
    {
        FirstName = fname;
        MiddleName = mname;
        LastName = lname;
        City = cityName;
        State = stateName;
    }

    // Return the first and last name.
    public void Deconstruct(out string fname, out string lname)
    {
        fname = FirstName;
        lname = LastName;
    }

    public void Deconstruct(out string fname, out string mname, out string lname)
    {
        fname = FirstName;
        mname = MiddleName;
        lname = LastName;
    }

    public void Deconstruct(out string fname, out string lname, 
                            out string city, out string state)
    {
        fname = FirstName;
        lname = LastName;
        city = City;
        state = State;
    }
}

public class Example
{
    public static void Main()
    {
        var p = new Person("John", "Quincy", "Adams", "Boston", "MA");

        // <Snippet1>
        // Deconstruct the person object.
        var (fName, _, city, _) = p;
        Console.WriteLine($"Hello {fName} of {city}!");
        // The example displays the following output:
        //      Hello John of Boston!
        // </Snippet1>
    }
}
// The example displays the following output:
//    Hello John Adams of Boston, MA!
```
</details>

### 使用 switch 和 is 的模式匹配
```csharp
using System;
using System.Globalization;

public class Example
{
   public static void Main()
   {
      object[] objects = { CultureInfo.CurrentCulture, 
                           CultureInfo.CurrentCulture.DateTimeFormat, 
                           CultureInfo.CurrentCulture.NumberFormat,
                           new ArgumentException(), null };
      foreach (var obj in objects)
         ProvidesFormatInfo(obj);
   }

   private static void ProvidesFormatInfo(object obj)         
   {
      switch (obj)
      {
         case IFormatProvider fmt:
            Console.WriteLine($"{fmt} object");
            break;
         case null:
            Console.Write("A null object reference: ");
            Console.WriteLine("Its use could result in a NullReferenceException");
            break;
         case object _:
            Console.WriteLine("Some object type without format information");
            break;
      }
   }
}
```
### 使用 out 参数调用方法
>因为该示例侧重验证日期字符串，而不是解析它来提取日期，所以方法的 out 参数为占位符。
```csharp
using System;

public class Example
{
   public static void Main()
   {
      string[] dateStrings = {"05/01/2018 14:57:32.8", "2018-05-01 14:57:32.8",
                              "2018-05-01T14:57:32.8375298-04:00", "5/01/2018",
                              "5/01/2018 14:57:32.80 -07:00", 
                              "1 May 2018 2:57:32.8 PM", "16-05-2018 1:00:32 PM", 
                              "Fri, 15 May 2018 20:10:57 GMT" };
      foreach (string dateString in dateStrings)
      {
         if (DateTime.TryParse(dateString, out _)) 
            Console.WriteLine($"'{dateString}': valid");
         else
            Console.WriteLine($"'{dateString}': invalid");
      }
   }
}
```
### 独立弃元
>可使用独立弃元来指示要忽略的任何变量
```csharp
using System;
using System.Threading.Tasks;

public class Example
{
   public static void Main()
   {
      ExecuteAsyncMethods().Wait();
   }

   private static async Task ExecuteAsyncMethods()
   {    
      Console.WriteLine("About to launch a task...");
      _ = Task.Run(() => { var iterations = 0;  
                           for (int ctr = 0; ctr < int.MaxValue; ctr++)
                              iterations++;
                           Console.WriteLine("Completed looping operation...");
                           throw new InvalidOperationException();
                         });
      await Task.Delay(5000);                        
      Console.WriteLine("Exiting after 5 second delay");
   }
}
```
>_ 也是有效标识符。 当在支持的上下文之外使用时，_ 不视为占位符，而视为有效变量。 如果名为 _ 的标识符已在范围内，则使用 _ 作为独立占位符可能导致
```csharp
private static void ShowValue(int _)
{
   byte[] arr = { 0, 0, 1, 2 };
   _ = BitConverter.ToInt32(arr, 0);
   Console.WriteLine(_);
}
// The example displays the following output:
//    
```
```csharp
private static bool RoundTrips(int _)
{
   string value = _.ToString();
   int newValue = 0;
   _ = Int32.TryParse(value, out newValue);
   return _ == newValue;
}
// The example displays the following compiler error:
//      error CS0029: Cannot implicitly convert type 'bool' to 'int'   
```
## <span style="color:#0366d6;">模式匹配</span>
### is 类型模式表达式
>过使用 is 表达式的扩展在测试成功时对变量赋值
```csharp
public static double ComputeAreaModernIs(object shape)
{
    if (shape is Square s)
        return s.Side * s.Side;
    else if (shape is Circle c)
        return c.Radius * c.Radius * Math.PI;
    else if (shape is Rectangle r)
        return r.Height * r.Length;
    // elided
    throw new ArgumentException(
        message: "shape is not a recognized shape",
        paramName: nameof(shape));
}
```
### 匹配 switch 语句
>switch 语句支持的唯一模式是常量模式。 它进一步限制为数字类型和 string 类型。 这些限制已移除，现在可以使用类型模式编写 switch 语句
```csharp
public static double ComputeAreaModernSwitch(object shape)
{
    switch (shape)
    {
        case Square s:
            return s.Side * s.Side;
        case Circle c:
            return c.Radius * c.Radius * Math.PI;
        case Rectangle r:
            return r.Height * r.Length;
        default:
            throw new ArgumentException(
                message: "shape is not a recognized shape",
                paramName: nameof(shape));
    }
}
```
### case 表达式中的 when 语句
>可以通过对 case 标签使用 when 子句，为面积为 0 的那些形状创建特殊 case。 边长为 0 的正方形，或半径为 0 的圆形的面积为 0。 可通过对 case 标签使用 when 语句来指定该条件
```csharp
public static double ComputeArea_Version3(object shape)
{
    switch (shape)
    {
        case Square s when s.Side == 0:
        case Circle c when c.Radius == 0:
            return 0;

        case Square s:
            return s.Side * s.Side;
        case Circle c:
            return c.Radius * c.Radius * Math.PI;
        default:
            throw new ArgumentException(
                message: "shape is not a recognized shape",
                paramName: nameof(shape));
    }
}
```
### case 表达式中的 var 声明
>第一条规则是 var 声明遵循正常的类型推理规则：推理出类型是 switch 表达式的静态类型。 根据此规则，类型始终匹配。
第二个规则是，var 声明没有其他类型模式表达式中包含的 null 检查。 也就是说，变量可为 NULL，只有在这种情况下，才必须执行 NULL 检查。
这两个规则表示，在许多情况下，case 表达式中的 var 声明匹配与 default 表达式相同的条件。 因为任何非默认事例都优先于 default 事例，所以永远不会执行 default 事例
```csharp
static object CreateShape(string shapeDescription)
{
    switch (shapeDescription)
    {
        case "circle":
            return new Circle(2);

        case "square":
            return new Square(4);
        
        case "large-circle":
            return new Circle(12);

        case var o when (o?.Trim().Length ?? 0) == 0:
            // white space
            return null;
        default:
            return "invalid shape description";
    }            
}
```
## <span style="color:#0366d6;">ref</span>
### ref 返回值和 ref 局部变量示例
>通过调用 GetBookByTitle 方法，可按引用返回个别 book 对象。
<details>
<summary>展开查看 </summary>

```csharp
public class Book
{
    public string Author;
    public string Title;
}

public class BookCollection
{
    private Book[] books = { new Book { Title = "Call of the Wild, The", Author = "Jack London" },
                        new Book { Title = "Tale of Two Cities, A", Author = "Charles Dickens" }
                       };
    private Book nobook = null;

    public ref Book GetBookByTitle(string title)
    {
        for (int ctr = 0; ctr < books.Length; ctr++)
        {
            if (title == books[ctr].Title)
                return ref books[ctr];
        }
        return ref nobook;
    }

    public void ListBooks()
    {
        foreach (var book in books)
        {
            Console.WriteLine($"{book.Title}, by {book.Author}");
        }
        Console.WriteLine();
    }
}
```
</details>

>调用方将 GetBookByTitle 方法所返回的值存储为 ref 局部变量时，调用方对返回值所做的更改将反映在 BookCollection 对象中，如下例所示
```csharp
var bc = new BookCollection();
bc.ListBooks();

ref var book = ref bc.GetBookByTitle("Call of the Wild, The");
if (book != null)
    book = new Book { Title = "Republic, The", Author = "Plato" };
bc.ListBooks();
// The example displays the following output:
//       Call of the Wild, The, by Jack London
//       Tale of Two Cities, A, by Charles Dickens
//       
//       Republic, The, by Plato
//       Tale of Two Cities, A, by Charles Dickens
```
### Ref readonly 局部变量
>Ref readonly 局部变量用于指代在其签名中具有 ref readonly 并使用 return ref 的方法或属性返回的值。 ref readonly 变量将 ref 本地变量的属性与 readonly 变量结合使用：它是所分配到的存储的别名，且无法修改
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```
### Ref 结构类型
>将 ref 修饰符添加到 struct 声明定义了该类型的实例必须为堆栈分配。 换言之，永远不能在作为另一类的成员的堆上创建这些类型的实例。

- 不能对ref struct装箱。无法向属于object、dynamic或任何接口类型的变量分配 ref struct 类型。
- ref struct 类型不能实现接口。
- 不能将 ref struct 声明为类或常规结构的字段成员。这包括声明自动实现的属性，后者会创建一个由编译器生成的支持字段。
- 不能声明异步方法中属于 ref struct 类型的本地变量。不能在返回类似Task、Task<TResult>或Task 类型的同  步方法中声明它们。
- 无法在迭代器中声明ref struct本地变量。
- 无法捕获Lambda表达式或本地函数中的ref struct变量。

>可以组合修饰符以将结构声明为 readonly ref。 readonly ref struct 兼具 ref struct 和 readonly struct 

# <span style="color:#0366d6;">C#7.1语法</span>
## <span style="color:#0366d6;">异步 main 方法</span>
>异步 Main 方法使你能够在 Main 方法中使用 await 关键字。 在过去，需要编写
```csharp
static int Main()
{
    return DoAsyncWork().GetAwaiter().GetResult();
}
```
>现在
```csharp
static async Task<int> Main()
{
    // This could also be replaced with the body
    // DoAsyncWork, including its await expressions:
    return await DoAsyncWork();
}
```
## <span style="color:#0366d6;">泛型类型参数的模式匹配</span>
>自 C# 7.1 起，is 和 switch 类型模式的模式表达式的类型可能为泛型类型参数。 这可能在检查 struct 或 class 类型且要避免装箱时最有用。
```csharp
void M<T1, T2>(T1 t1, T2 t2)
{
    switch (t2)
    {
        case T1 _:
            break;
        case T2 _:
            break;
        default:
            break;
    }
}
```
## <span style="color:#0366d6;">元组</span>
### 元组名称投影
>元组名称投影，如果未提供显式名称，上述名称将优先于任何投影的名称。
以下使用名称explicitFieldOne和explicitFieldTwo而不是localVariableOne和localVariableTwo
```csharp
var localVariableOne = 5;
var localVariableTwo = "some text";

var tuple = (explicitFieldOne: localVariableOne, explicitFieldTwo: localVariableTwo);

```
>以下初始化表达式具有字段名称 Item1其值为 42和 stringContent（其值为“The answer to everything”）

```csharp
var stringContent = "The answer to everything";
var mixedTuple = (42, stringContent);         
```
## <span style="color:#0366d6;">默认文本表达式</span>
>默认文本表达式是针对默认值表达式的一项增强功能。 这些表达式将变量初始化为默认值。 过去会这么编写
```csharp
Func<string, bool> whereClause = default(Func<string, bool>);
```
>现在，可以省略掉初始化右侧的类型：
```csharp
Func<string, bool> whereClause = default;
```
## <span style="color:#0366d6;">引用程序集生成</span>
>有两个新编译器选项可生成仅引用程序集：-refout 和 -refonly
# <span style="color:#0366d6;">C#7.2语法</span>
## <span style="color:#0366d6;">非尾随命名参数</span>
>没有后接任何位置实参或
```csharp
以 C# 7.2 开头，则它们就有效并用在正确位置 。 在以下示例中，形参 orderNum 位于正确的位置，但未显式命名。
```
>以 C# 7.2 开头，则它们就有效并用在正确位置 。 在以下示例中，形参 orderNum 位于正确的位置，但未显式命名。
```csharp
PrintOrderDetails(sellerName: "Gift Shop", 31, productName: "Red Mug");
```
>遵循任何无序命名参数的位置参数无效。
```csharp
// This generates CS1738: Named argument specifications must appear after all fixed arguments have been specified.
PrintOrderDetails(productName: "Red Mug", 31, "Gift Shop");
```
## <span style="color:#0366d6;">in</span>
>作为 in 参数传递的变量在方法调用中传递之前必须进行初始化。 但是，所调用的方法可能不会分配值或修改参数。
in 参数修饰符可在 C# 7.2 及更高版本中使用。 以前的版本生成编译器错误 CS8107（“‘readonly 引用’功能在 C# 7.0 中不可用。 请使用语言版本 7.2 或更高版本。”）

>通过理解使用 in 参数的动机，可以理解使用按值方法和使用 in 参数方法的重载决策规则。 定义使用 in 参数的方法是一项潜在的性能优化。 某些 struct 类型参数可能很大，在紧凑的循环或关键代码路径中调用方法时，复制这些结构的成本就很高。 方法声明 in 参数以指定参数可能按引用安全传递，因为所调用的方法不修改该参数的状态。 按引用传递这些参数可以避免（可能产生的）高昂的复制成本。

```csharp
static void Method(int argument)
{
    // implementation removed
}

static void Method(in int argument)
{
    // implementation removed
}

Method(5); // Calls overload passed by value
Method(5L); // CS1503: no implicit conversion from long to int
short s = 0;
Method(s); // Calls overload passed by value.
Method(in s); // CS1503: cannot convert from in short to in int
int i = 42;
Method(i); // Calls overload passed by value
Method(in i); // passed by readonly reference, explicitly using `in`
```
## <span style="color:#0366d6;">private protected 访问修饰符</span>
>新的复合访问修饰符：private protected 指示可通过包含同一程序集中声明的类或派生类来访问成员。 虽然 protected internal 允许通过同一程序集中的类或派生类进行访问，但 private protected 限制对同一程序集中声明的派生类的访问。
```csharp
// Assembly1.cs  
// Compile with: /target:library  
public class BaseClass
{
    private protected int myValue = 0;
}

public class DerivedClass1 : BaseClass
{
    void Access()
    {
        var baseObject = new BaseClass();

        // Error CS1540, because myValue can only be accessed by
        // classes derived from BaseClass.
        // baseObject.myValue = 5;  

        // OK, accessed through the current derived class instance
        myValue = 5;
    }
}
```
```csharp
// Assembly2.cs  
// Compile with: /reference:Assembly1.dll  
class DerivedClass2 : BaseClass
{
    void Access()
    {
        // Error CS0122, because myValue can only be
        // accessed by types in Assembly1
        // myValue = 10;
    }
}
```
## <span style="color:#0366d6;">数值文字中的前导下划线</span>
>C# 7.0 中实现了对数字分隔符的支持，但这不允许文字值的第一个字符是 _。 十六进制文本和二进制文件现可以 _ 开头。
```csharp
int binaryValue = 0b_0101_0101;
```
## <span style="color:#0366d6;">条件 ref 表达式</span>
>条件表达式可能生成 ref 结果而不是值
```csharp
ref var r = ref (arr != null ? ref arr[0] : ref otherArr[0]);
```
## <span style="color:#0366d6;">安全高效的代码的增强功能</span>
>todo
## <span style="color:#0366d6;">C# 7.3</span>
### <span style="color:#0366d6;">相等和元组</span>
>从 C# 7.3 开始，元组类型支持 == 和 != 运算符。 这些运算符按顺序将左边参数的每个成员与右边参数的每个成员进行比较。 这些比较将发生短路。 只要有一对不相等，它们即会停止计算成员。 以下代码示例使用 ==，但比较规则均适用于 !=。
```csharp
var left = (a: 5, b: 10);
var right = (a: 5, b: 10);
Console.WriteLine(left == right); // displays 'true'
```
>元组成员名称不参与相等测试。 但是，如果其中一个操作数是含有显式名称的元组文本，则当这些名称与其他操作数的名称不匹配时，编译器将生成警告 CS8383。 在两个操作数都为元组文本的情况下，警告位于右侧操作数，
```csharp
(int a, string b) pair = (1, "Hello");
(int z, string y) another = (1, "Hello");
Console.WriteLine(pair == another); // true. Member names don't participate.
Console.WriteLine(pair == (z: 1, y: "Hello")); // warning: literal contains different member nam
```
### <span style="color:#0366d6;">其它todo</span>


