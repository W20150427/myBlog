---
title: C#4.0语法
date: 2020-02-25
tags: [程序设计语言，C#]
categories: csharp程序设计语言
---
C#4.0语法（发布于2010，.NET Framework 4）
<!-- more -->
参考：<https://docs.microsoft.com/zh-cn/dotnet/csharp/>
# <span style="color:#0366d6;">C#4.0语法</span>
## <span style="color:#0366d6;">动态绑定</span>
>dynamic 类型表示变量的使用和对其成员的引用绕过编译时类型检查。 改为在运行时解析这些操作

<details>
<summary>展开查看</summary>

```csharp
namespace DynamicExamples
{
    class Program
    {
        static void Main(string[] args)
        {
            ExampleClass ec = new ExampleClass();
            Console.WriteLine(ec.exampleMethod(10));
            Console.WriteLine(ec.exampleMethod("value"));

            // The following line causes a compiler error because exampleMethod
            // takes only one argument.
            //Console.WriteLine(ec.exampleMethod(10, 4));

            dynamic dynamic_ec = new ExampleClass();
            Console.WriteLine(dynamic_ec.exampleMethod(10));

            // Because dynamic_ec is dynamic, the following call to exampleMethod
            // with two arguments does not produce an error at compile time.
            // However, it does cause a run-time error. 
            //Console.WriteLine(dynamic_ec.exampleMethod(10, 4));
        }
    }

    class ExampleClass
    {
        static dynamic field;
        dynamic prop { get; set; }

        public dynamic exampleMethod(dynamic d)
        {
            dynamic local = "Local variable";
            int two = 2;

            if (d is int)
            {
                return local;
            }
            else
            {
                return two;
            }
        }
    }
}
```
</details>

## <span style="color:#0366d6;">命名实参和可选实参</span>
>参数的默认值必须由以下几种表达式中的一种来赋予：
1.常量，例如文本字符串或数字。
2.new ValType() 形式的表达式，其中 ValType 是值类型。 请注意，这会调用该值类型的隐式无参数构造函数，该函数不是类型的实际成员。
3.default(ValType) 形式的表达式，其中 ValType 是值类型。

```csharp
public void ExampleMethod(int required, int optionalInt = default(int),
                             string description = "Optional Description")
   {
      Console.WriteLine("{0}: {1} + {2} = {3}", description, required, 
                        optionalInt, required + optionalInt);
   }
```
>前两个方法调用使用位置自变量。 第一个方法同时省略了两个可选自变量，而第二个省略了最后一个自变量。 第三个方法调用向必需的参数提供位置自变量，但使用命名的自变量向 description 参数提供值，同时省略 optionalInt 自变量
```csharp
var opt = new Options();
    opt.ExampleMethod(10);
    opt.ExampleMethod(10, 2);
    opt.ExampleMethod(12, description: "Addition with zero:");
```
## <span style="color:#0366d6;">泛型中的协变和逆变</span>
>协变和逆变都是术语，前者指能够使用比原始指定的派生类型的派生程度更大（更具体的）的类型，后者指能够使用比原始指定的派生类型的派生程度更小（不太具体的）的类型。

```csharp
using System;

public class Type1 {}
public class Type2 : Type1 {}
public class Type3 : Type2 {}

public class Program
{
    public static Type3 MyMethod(Type1 t)
    {
        return t as Type3 ?? new Type3();
    }

    static void Main() 
    {
        Func<Type2, Type2> f1 = MyMethod;

        // Covariant return type and contravariant parameter type.
        Func<Type3, Type1> f2 = f1;
        Type1 t1 = f2(new Type3());
    }
}
```
### 具有协变类型参数的泛型接口
<details>
<summary>展开查看</summary>

```csharp
using System;
using System.Collections.Generic;

class Base
{
    public static void PrintBases(IEnumerable<Base> bases)
    {
        foreach(Base b in bases)
        {
            Console.WriteLine(b);
        }
    }
}

class Derived : Base
{
    public static void Main()
    {
        List<Derived> dlist = new List<Derived>();

        Derived.PrintBases(dlist);
        IEnumerable<Base> bIEnum = dlist;
    }
}
```
</details>

### 具有逆变泛型类型参数的泛型接口
<details>
<summary>展开查看</summary>

```csharp
using System;
using System.Collections.Generic;

abstract class Shape
{
    public virtual double Area { get { return 0; }}
}

class Circle : Shape
{
    private double r;
    public Circle(double radius) { r = radius; }
    public double Radius { get { return r; }}
    public override double Area { get { return Math.PI * r * r; }}
}

class ShapeAreaComparer : System.Collections.Generic.IComparer<Shape>
{
    int IComparer<Shape>.Compare(Shape a, Shape b) 
    { 
        if (a == null) return b == null ? 0 : -1;
        return b == null ? 1 : a.Area.CompareTo(b.Area);
    }
}

class Program
{
    static void Main()
    {
        // You can pass ShapeAreaComparer, which implements IComparer<Shape>,
        // even though the constructor for SortedSet<Circle> expects 
        // IComparer<Circle>, because type parameter T of IComparer<T> is
        // contravariant.
        SortedSet<Circle> circlesByArea = 
            new SortedSet<Circle>(new ShapeAreaComparer()) 
                { new Circle(7.2), new Circle(100), null, new Circle(.01) };

        foreach (Circle c in circlesByArea)
        {
            Console.WriteLine(c == null ? "null" : "Circle with area " + c.Area);
        }
    }
}

/* This code example produces the following output:

null
Circle with area 0.000314159265358979
Circle with area 162.860163162095
Circle with area 31415.9265358979
 */
```
</details>

### 定义 Variant 泛型接口和委托
>协变类型参数用 out 关键字,逆变类型参数用 in 关键字

