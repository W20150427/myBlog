---
title: C#3.0语法
date: 2020-02-24
tags: [程序设计语言，C#]
categories: csharp程序设计语言
---
C#3.0语法（发布于2007年下半年，.NET Framework 3.5）
<!-- more -->
参考：<https://docs.microsoft.com/zh-cn/dotnet/csharp/>
# <span style="color:#0366d6;">C#3.0语法</span>
## <span style="color:#0366d6;">推断类型</span>
```csharp
var i = 10; // Implicitly typed.
int i = 10; // Explicitly typed.
```
## <span style="color:#0366d6;">匿名类型</span>
>匿名类型提供了一种方便的方法，可用来将一组只读属性封装到单个对象中，而无需首先显式定义一个类型。 类型名由编译器生成，并且不能在源代码级使用。 每个属性的类型由编译器推断
```csharp
var v = new { Amount = 108, Message = "Hello" };  
  
// Rest the mouse pointer over v.Amount and v.Message in the following  
// statement to verify that their inferred types are int and string.  
Console.WriteLine(v.Amount + v.Message);
```
>匿名类型通常用在查询表达式的 select 子句中，以便返回源序列中每个对象的属性子集
```csharp
var productQuery = 
    from prod in products
    select new { prod.Color, prod.Price };

foreach (var v in productQuery)
{
    Console.WriteLine("Color={0}, Price={1}", v.Color, v.Price);
}
```
## <span style="color:#0366d6;">扩展方法</span>
>扩展方法被定义为静态方法，但它们是通过实例方法语法进行调用的。 它们的第一个参数指定该方法作用于哪个类型，并且该参数以 this 修饰符为前缀。 仅当你使用 using 指令将命名空间显式导入到源代码中之后，扩展方法才位于范围中。
```csharp
namespace ExtensionMethods
{
    public static class MyExtensions
    {
        public static int WordCount(this String str)
        {
            return str.Split(new char[] { ' ', '.', '?' }, 
                             StringSplitOptions.RemoveEmptyEntries).Length;
        }
    }   
}
```

```csharp
using ExtensionMethods; 
string s = "Hello Extension Methods";  
int i = s.WordCount();
```
## <span style="color:#0366d6;">分部方法</span>
>分部方法在分部类型的一部分中定义了签名，并在该类型的另一部分中定义了实现。 通过分部方法，类设计器可提供与事件处理程序类似的方法挂钩，以便开发者决定是否实现。
1.分部类型各部分中的签名必须匹配。
2.方法必须返回 void。
3.不允许使用访问修饰符。 分部方法是隐式专用的。

```csharp
namespace PM
{
    partial class A
    {
        partial void OnSomethingHappened(string s);
    }

    // This part can be in a separate file.
    partial class A
    {
        // Comment out this method and the program
        // will still compile.
        partial void OnSomethingHappened(String s)
        {
            Console.WriteLine("Something happened: {0}", s);
        }
    }
}
```
## <span style="color:#0366d6;">自动实现的属性</span>
>C# 3.0 及更高版本，当属性访问器中不需要任何其他逻辑时，自动实现的属性会使属性声明更加简洁
```csharp
// This class is mutable. Its data can be modified from
// outside the class.
class Customer
{
    // Auto-implemented properties for trivial get and set
    public double TotalPurchases { get; set; }
    public string Name { get; set; }
    public int CustomerID { get; set; }

    // Constructor
    public Customer(double purchases, string name, int ID)
    {
        TotalPurchases = purchases;
        Name = name;
        CustomerID = ID;
    }

    // Methods
    public string GetContactInfo() { return "ContactInfo"; }
    public string GetTransactionHistory() { return "History"; }

    // .. Additional methods, events, etc.
}
```
## <span style="color:#0366d6;">对象和集合初始值设定</span>
### 对象初始值设定
```csharp
public class Cat
{
    // Auto-implemented properties.
    public int Age { get; set; }
    public string Name { get; set; }

    public Cat()
    {
    }

    public Cat(string name)
    {
        this.Name = name;
    }
}
```
```csharp
Cat cat = new Cat { Age = 10, Name = "Fluffy" };
Cat sameCat = new Cat("Fluffy"){ Age = 10 };
```
### 匿名类型的对象初始值
```csharp
var pet = new { Age = 10, Name = "Fluffy" }; 
```
### 集合初始值
```csharp
List<Cat> cats = new List<Cat>
{
    new Cat{ Name = "Sylvester", Age=8 },
    new Cat{ Name = "Whiskers", Age=2 },
    new Cat{ Name = "Sasha", Age=14 }
};
```
## <span style="color:#0366d6;">Lambda 表达式</span>
>表达式 lambda，表达式为其主体
```csharp
(input-parameters) => expression
```
>语句 lambda，语句块作为其主体

```csharp
(input-parameters) => { <sequence-of-statements> }
```
### 含标准查询运算符的 lambda
>有 2 个参数且不返回值的 Lambda 表达式可转换为 Action<T1,T2> 委托。有 1 个参数且返回值的 Lambda 表达式可转换为 Func<T,TResult> 委托
### Lambda 表达式中的类型推理
>编写 lambda 时，通常不必为输入参数指定类型，因为编译器可以根据 lambda 主体、参数类型以及 C# 语言规范中描述的其他因素来推断类型。 对于大多数标准查询运算符，第一个输入是源序列中的元素类型。

>如果要查询 IEnumerable<Customer>，则输入变量将被推断为 Customer 对象，这意味着你可以访问其方法和属性
```csharp
customers.Where(c => c.City == "London");
```
>1.Lambda 包含的参数数量必须与委托类型包含的参数数量相同。
 2.Lambda 中的每个输入参数必须都能够隐式转换为其对应的委托参数。
 3.Lambda 的返回值（如果有）必须能够隐式转换为委托的返回类型。
 ## <span style="color:#0366d6;">LINQ</span>
 >下面代码展示的是linq的查询语句和扩展方法

 <details>
 <summary>展开查看</summary>

```csharp
class QueryVMethodSyntax
{
    static void Main()
    {
        int[] numbers = { 5, 10, 8, 3, 6, 12};

        //Query syntax:
        IEnumerable<int> numQuery1 = 
            from num in numbers
            where num % 2 == 0
            orderby num
            select num;

        //Method syntax:
        IEnumerable<int> numQuery2 = numbers.Where(num => num % 2 == 0).OrderBy(n => n);

        foreach (int i in numQuery1)
        {
            Console.Write(i + " ");
        }
        Console.WriteLine(System.Environment.NewLine);
        foreach (int i in numQuery2)
        {
            Console.Write(i + " ");
        }
        
        // Keep the console open in debug mode.
        Console.WriteLine(System.Environment.NewLine);
        Console.WriteLine("Press any key to exit");
        Console.ReadKey();
    }
}
/*
    Output:
    6 8 10 12
    6 8 10 12
 */
 ```
</details>

## <span style="color:#0366d6;">表达式树</span>
>todo
 