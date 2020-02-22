---
title: C#2.0语法
date: 2020-02-22
tags: [程序设计语言，C#]
categories: csharp程序设计语言
---
C#2.0语法（发布于2005）
<!-- more -->
参考：<https://docs.microsoft.com/zh-cn/dotnet/csharp/>
## <span style="color:#0366d6;">C# 2.0</span>
## <span style="color:#0366d6;">getter/setter 单独可访问性</span>
>就是在get或者set前面可以加访问修饰符
```csharp
public class User
{
    public User(int age)
    {
      this.age=age;
    }
    public int age;
    public Age {
      get {return age;}
      private set{age=value;}
    }
}
```
## <span style="color:#0366d6;">静态类</span>
>以下列表提供静态类的主要功能：
1.只包含静态成员。
2.无法进行实例化。
3.会进行密封。
4.不能包含实例构造函数。
静态类会进行密封，因此不能继承。 它们不能继承自任何类（除了 Object）。 静态类不能包含实例构造函数；但是，它们可以包含静态构造函数。
```csharp
public static class TemperatureConverter
{
    public static double CelsiusToFahrenheit(string temperatureCelsius)
    {
        // Convert argument to double for calculations.
        double celsius = Double.Parse(temperatureCelsius);

        // Convert Celsius to Fahrenheit.
        double fahrenheit = (celsius * 9 / 5) + 32;

        return fahrenheit;
    }

    public static double FahrenheitToCelsius(string temperatureFahrenheit)
    {
        // Convert argument to double for calculations.
        double fahrenheit = Double.Parse(temperatureFahrenheit);

        // Convert Fahrenheit to Celsius.
        double celsius = (fahrenheit - 32) * 5 / 9;

        return celsius;
    }
}
```
## <span style="color:#0366d6;">可为空的值类型</span>
### 声明和赋值
>由于值类型可隐式转换为相应的可为空的值类型，因此可以像向其基础值类型赋值一样，向可为空值类型的变量赋值。 还可分配 null 值。
```csharp
double? pi = 3.14;
char? letter = 'a';

int m2 = 10;
int? m = m2;

bool? flag = null;

// An array of a nullable type:
int?[] arr = new int?[10];
```
### 提升的运算符
>预定义的一元运算符和二元运算符或值类型 T 支持的任何重载运算符也受相应的可为空值类型 T? 支持。 如果一个或全部两个操作数为 null ，则这些运算符（也称为提升的运算符）将生成 null；否则，运算符使用其操作数所包含的值来计算结果。
```csharp
int? a = 10;
int? b = null;
int? c = 10;

a++;        // a is 11
a = a * c;  // a is 110
a = a + b;  // a is null
```
>对于比较运算符 <、>、<= 和 >=，如果一个或全部两个操作数都为 null，则结果为 false；否则，将比较操作数的包含值。 请勿作出如下假定：由于某个特定的比较（例如 <=）返回 false，则相反的比较 (>) 返回 true
```csharp
int? a = 10;
Console.WriteLine($"{a} >= null is {a >= null}");
Console.WriteLine($"{a} < null is {a < null}");
Console.WriteLine($"{a} == null is {a == null}");
// Output:
// 10 >= null is False
// 10 < null is False
// 10 == null is False
```
>对于相等运算符 ==，如果两个操作数都为 null，则结果为 true；如果只有一个操作数为 null，则结果为 false；否则，将比较操作数的包含值。
对于不等运算符 !=，如果两个操作数都为 null，则结果为 false；如果只有一个操作数为 null，则结果为 true；否则，将比较操作数的包含值。
```csharp
int? b = null;
int? c = null;
Console.WriteLine($"null >= null is {b >= c}");
Console.WriteLine($"null == null is {b == c}");
// Output:
// null >= null is False
// null == null is True
```
### 装箱和取消装箱
>可为空值类型的实例 T?已装箱，如下所示
1.如果 HasValue 返回 false，则生成空引用
2.如果 HasValue 返回 true，则基础值类型 T 的对应值将装箱，而不对 Nullable \<T\> 的实例进行装箱
可将值类型 T 的已装箱值取消装箱到相应的可为空值类型 T?
```csharp
int a = 41;
object aBoxed = a;
int? aNullable = (int?)aBoxed;
Console.WriteLine($"Value of aNullable: {aNullable}");

object aNullableBoxed = aNullable;
if (aNullableBoxed is int valueOfA)
{
    Console.WriteLine($"aNullableBoxed is boxed int: {valueOfA}");
}
// Output:
// Value of aNullable: 41
// aNullableBoxed is boxed int: 41
```
## <span style="color:#0366d6;">部分类型</span>
>拆分一个类、一个结构、一个接口或一个方法的定义到两个或更多的文件中是可能的
partial 修饰符不可用于委托或枚举声明中
```csharp
public partial class Employee
{
    public void DoWork()
    {
    }
}

public partial class Employee
{
    public void GoToLunch()
    {
    }
}
```
## <span style="color:#0366d6;">匿名方法</span>
>delegate 运算符创建一个可以转换为委托类型的匿名方法
```csharp
Func<int, int, int> sum = delegate (int a, int b) { return a + b; };
Console.WriteLine(sum(3, 4));  // output: 7
```
## <span style="color:#0366d6;">迭代器</span>
>yield”不是保留字，只有在 return 或 break 关键字之前使用时才有特殊含义
迭代器 可用于逐步迭代集合，例如列表和数组。迭代器方法或 get 访问器可对集合执行自定义迭代。 迭代器方法使用 yield return 语句返回元素，每次返回一个。 到达 yield return 语句时，会记住当前在代码中的位置。 下次调用迭代器函数时，将从该位置重新开始执行
```csharp
static void Main()
{
    foreach (int number in SomeNumbers())
    {
        Console.Write(number.ToString() + " ");
    }
    // Output: 3 5 8
    Console.ReadKey();
}

public static System.Collections.IEnumerable SomeNumbers()
{
    yield return 3;
    yield return 5;
    yield return 8;
}
```
>迭代器方法或 get 访问器的返回类型可以是IEnumerable、IEnumerable\<T\>、IEnumerator或IEnumerator\<T\>可以使用 yield break 语句来终止迭代
迭代器可用作一种方法，或一个 get 访问器。 不能在<span style="color:#0065b3;">事件、实例构造函数、静态构造函数或静态终结器</span>中使用迭代器。
必须存在从 yield return 语句中的表达式类型到迭代器返回的 IEnumerable<T\> 类型参数的隐式转换。
即使将迭代器编写成方法，编译器也会将其转换为实际上是状态机的嵌套类。可使用 Ildasm.exe 工具查看
<details>
<summary>点开查看</summary>

```csharp

public class Zoo : IEnumerable
{
    // Private members.
    private List<Animal> animals = new List<Animal>();

    // Public methods.
    public void AddMammal(string name)
    {
        animals.Add(new Animal { Name = name, Type = Animal.TypeEnum.Mammal });
    }

    public void AddBird(string name)
    {
        animals.Add(new Animal { Name = name, Type = Animal.TypeEnum.Bird });
    }

    public IEnumerator GetEnumerator()
    {
        foreach (Animal theAnimal in animals)
        {
            yield return theAnimal.Name;
        }
    }

    // Public members.
    public IEnumerable Mammals
    {
        get { return AnimalsForType(Animal.TypeEnum.Mammal); }
    }

    public IEnumerable Birds
    {
        get { return AnimalsForType(Animal.TypeEnum.Bird); }
    }

    // Private methods.
    private IEnumerable AnimalsForType(Animal.TypeEnum type)
    {
        foreach (Animal theAnimal in animals)
        {
            if (theAnimal.Type == type)
            {
                yield return theAnimal.Name;
            }
        }
    }

    // Private class.
    private class Animal
    {
        public enum TypeEnum { Bird, Mammal }

        public string Name { get; set; }
        public TypeEnum Type { get; set; }
    }
}
```
</details>

>引用类实例 (theZoo) 的 foreach 语句隐式调用 GetEnumerator 方法。 引用 Birds 和 Mammals 属性的 foreach 语句使用 AnimalsForType 命名迭代器方法。
```csharp
static void Main()
{
    Zoo theZoo = new Zoo();

    theZoo.AddMammal("Whale");
    theZoo.AddMammal("Rhinoceros");
    theZoo.AddBird("Penguin");
    theZoo.AddBird("Warbler");

    foreach (string name in theZoo)
    {
        Console.Write(name + " ");
    }
    Console.WriteLine();
    // Output: Whale Rhinoceros Penguin Warbler

    foreach (string name in theZoo.Birds)
    {
        Console.Write(name + " ");
    }
    Console.WriteLine();
    // Output: Penguin Warbler

    foreach (string name in theZoo.Mammals)
    {
        Console.Write(name + " ");
    }
    Console.WriteLine();
    // Output: Whale Rhinoceros

    Console.ReadKey();
}
```










