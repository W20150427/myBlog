---
title: C#2.0语法
date: 2020-02-22
tags: [程序设计语言，C#]
categories: csharp程序设计语言
---
C#2.0语法（发布于2005，.NET Framework 2.0 .NET Framework 3.0）

<!-- more -->
参考：<https://docs.microsoft.com/zh-cn/dotnet/csharp/>
# <span style="color:#0366d6;">C# 2.0</span>
## <span style="color:#0366d6;">:: 运算符</span>
>使用命名空间别名限定符 :: 访问已设置别名的命名空间的成员。
>使用 using 别名指令创建的命名空间别名
```csharp
using forwinforms = System.Drawing;
using forwpf = System.Windows;

public class Converters
{
    public static forwpf::Point Convert(forwinforms::Point point) => new forwpf::Point(point.X, point.Y);
}
```
>外部别名,有时你可能不得不引用具有相同的完全限定类型名称的程序集的两个版本
GridV1::Grid 是 grid.dll 中的网格控件，GridV2::Grid 是 grid20.dll 中的网格控件
```csharp
/r:GridV1=grid.dll

/r:GridV2=grid20.dll

extern alias GridV1;

extern alias GridV2;
```
>global 别名，该别名是全局命名空间别名。与 :: 限定符一起使用时，global 别名始终引用全局命名空间，即使存在用户定义的 global 命名空间别名也是如此
以下示例使用 global 别名访问 .NET System 命名空间，该命名空间是全局命名空间的成员。如果没有 global 别名，则将访问用户定义的 System 命名空间（该命名空间是 MyCompany.MyProduct 命名空间的成员）
```csharp
namespace MyCompany.MyProduct.System
{
    class Program
    {
        static void Main() => global::System.Console.WriteLine("Using global alias");
    }

    class Console
    {
        string Suggestion => "Consider renaming this class";
    }
}
```
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
## <span style="color:#0366d6;">协变和逆变</span>
>协变和逆变能够实现数组类型、委托类型和泛型类型参数的隐式引用转换。协变保留分配兼容性，逆变则与之相反。
>数组协变和逆变
```csharp
public class First { }  
public class Second : First { }  
public delegate First SampleDelegate(Second a);  
```
>委托协变和逆变
```csharp
// Matching signature.  
public static First ASecondRFirst(Second second)  
{ return new First(); }  
  
// The return type is more derived.  
public static Second ASecondRSecond(Second second)  
{ return new Second(); }  
  
// The argument type is less derived.  
public static First AFirstRFirst(First first)  
{ return new First(); }  
  
// The return type is more derived   
// and the argument type is less derived.  
public static Second AFirstRSecond(First first)  
{ return new Second(); }   
```

```csharp
// Assigning a method with a matching signature   
// to a non-generic delegate. No conversion is necessary.  
SampleDelegate dNonGeneric = ASecondRFirst;  
// Assigning a method with a more derived return type   
// and less derived argument type to a non-generic delegate.  
// The implicit conversion is used.  
SampleDelegate dNonGenericConversion = AFirstRSecond;  
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
>拆分<span style="color:#0065b3;">类、结构、接口、方法</span>的定义到两个或更多的文件中是可能的
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
>yield不是保留字，只有在 return 或 break 关键字之前使用时才有特殊含义
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
迭代器可用作一种方法，或一个 get 访问器。不能在<span style="color:#0065b3;">事件、实例构造函数、静态构造函数或静态终结器</span>中使用迭代器。
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
<details>
<summary>点开查看</summary>

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
</details>

## <span style="color:#0366d6;">泛型</span>
### 泛型类
>泛型类封装不特定于特定数据类型的操作。泛型类最常见用法是用于链接列表、哈希表、堆栈、队列和树等集合。 无论存储数据的类型如何，添加项和从集合删除项等操作的执行方式基本相同。
```csharp
class BaseNode { }
class BaseNodeGeneric<T> { }

// concrete type
class NodeConcrete<T> : BaseNode { }
```
>继承封闭类型
```csharp
//closed constructed type
class NodeClosed<T> : BaseNodeGeneric<int> { }
```
>继承开放类型
```csharp
//open constructed type 
class NodeOpen<T> : BaseNodeGeneric<T> { }
```
>泛型类可继承自具体的封闭式构造或开放式构造基类
```csharp
//No error
class Node1 : BaseNodeGeneric<int> { }

//Generates an error
//class Node2 : BaseNodeGeneric<T> {}

//Generates an error
//class Node3 : T {}
```
>继承自开放式的泛型类必须对非此继承任何基类类型参数提供类型参数
```csharp
class BaseNodeMultiple<T, U> { }

//No error
class Node4<T> : BaseNodeMultiple<T, int> { }

//No error
class Node5<T, U> : BaseNodeMultiple<T, U> { }

//Generates an error
//class Node6<T> : BaseNodeMultiple<T, U> {} 
```
>继承自开放式构造类型的泛型类必须指定作为基类型上约束
```csharp
class NodeItem<T> where T : System.IComparable<T>, new() { }
class SpecialNodeItem<T> : NodeItem<T> where T : System.IComparable<T>, new() { }
```
>开放式构造和封闭式构造类型可用作方法参数
```csharp
void Swap<T>(List<T> list1, List<T> list2)
{
    //code to swap items
}

void Swap(List<int> list1, List<int> list2)
{
    //code to swap items
}
```
>派生类可以扩展基类类型的个数
```csharp
class BaseNodeMultiple<T> { }

//No error
class Node4<T,U> : BaseNodeMultiple<T> { }

```
### 泛型接口
>接口被指定为类型参数上的约束时，仅可使用实现接口的类型
<details>
<summary>展开查看</summary>

```csharp
//Type parameter T in angle brackets.
public class GenericList<T> : System.Collections.Generic.IEnumerable<T>
{
    protected Node head;
    protected Node current = null;

    // Nested class is also generic on T
    protected class Node
    {
        public Node next;
        private T data;  //T as private member datatype

        public Node(T t)  //T used in non-generic constructor
        {
            next = null;
            data = t;
        }

        public Node Next
        {
            get { return next; }
            set { next = value; }
        }

        public T Data  //T as return type of property
        {
            get { return data; }
            set { data = value; }
        }
    }

    public GenericList()  //constructor
    {
        head = null;
    }

    public void AddHead(T t)  //T as method parameter type
    {
        Node n = new Node(t);
        n.Next = head;
        head = n;
    }

    // Implementation of the iterator
    public System.Collections.Generic.IEnumerator<T> GetEnumerator()
    {
        Node current = head;
        while (current != null)
        {
            yield return current.Data;
            current = current.Next;
        }
    }

    // IEnumerable<T> inherits from IEnumerable, therefore this class 
    // must implement both the generic and non-generic versions of 
    // GetEnumerator. In most cases, the non-generic method can 
    // simply call the generic method.
    System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}

public class SortedList<T> : GenericList<T> where T : System.IComparable<T>
{
    // A simple, unoptimized sort algorithm that 
    // orders list elements from lowest to highest:

    public void BubbleSort()
    {
        if (null == head || null == head.Next)
        {
            return;
        }
        bool swapped;

        do
        {
            Node previous = null;
            Node current = head;
            swapped = false;

            while (current.next != null)
            {
                //  Because we need to call this method, the SortedList
                //  class is constrained on IEnumerable<T>
                if (current.Data.CompareTo(current.next.Data) > 0)
                {
                    Node tmp = current.next;
                    current.next = current.next.next;
                    tmp.next = current;

                    if (previous == null)
                    {
                        head = tmp;
                    }
                    else
                    {
                        previous.next = tmp;
                    }
                    previous = tmp;
                    swapped = true;
                }
                else
                {
                    previous = current;
                    current = current.next;
                }
            }
        } while (swapped);
    }
}

// A simple class that implements IComparable<T> using itself as the 
// type argument. This is a common design pattern in objects that 
// are stored in generic lists.
public class Person : System.IComparable<Person>
{
    string name;
    int age;

    public Person(string s, int i)
    {
        name = s;
        age = i;
    }

    // This will cause list elements to be sorted on age values.
    public int CompareTo(Person p)
    {
        return age - p.age;
    }

    public override string ToString()
    {
        return name + ":" + age;
    }

    // Must implement Equals.
    public bool Equals(Person p)
    {
        return (this.age == p.age);
    }
}

class Program
{
    static void Main()
    {
        //Declare and instantiate a new generic SortedList class.
        //Person is the type argument.
        SortedList<Person> list = new SortedList<Person>();

        //Create name and age values to initialize Person objects.
        string[] names = new string[] 
        { 
            "Franscoise", 
            "Bill", 
            "Li", 
            "Sandra", 
            "Gunnar", 
            "Alok", 
            "Hiroyuki", 
            "Maria", 
            "Alessandro", 
            "Raul" 
        };

        int[] ages = new int[] { 45, 19, 28, 23, 18, 9, 108, 72, 30, 35 };

        //Populate the list.
        for (int x = 0; x < 10; x++)
        {
            list.AddHead(new Person(names[x], ages[x]));
        }

        //Print out unsorted list.
        foreach (Person p in list)
        {
            System.Console.WriteLine(p.ToString());
        }
        System.Console.WriteLine("Done with unsorted list");

        //Sort the list.
        list.BubbleSort();

        //Print out sorted list.
        foreach (Person p in list)
        {
            System.Console.WriteLine(p.ToString());
        }
        System.Console.WriteLine("Done with sorted list");
    }
}
```
</details>

>适用于类的继承规则也适用于接口

```csharp
interface IMonth<T> { }

interface IJanuary     : IMonth<int> { }  //No error
interface IFebruary<T> : IMonth<int> { }  //No error
interface IMarch<T>    : IMonth<T> { }    //No error
//interface IApril<T>  : IMonth<T, U> {}  //Error
```
>具体类可实现封闭式构造接口
```csharp
interface IBaseInterface<T> { }

class SampleClass : IBaseInterface<string> { }
```
>只要类形参列表提供接口所需的所有实参，泛型类即可实现泛型接口或封闭式构造接口
```csharp
interface IBaseInterface1<T> { }
interface IBaseInterface2<T, U> { }

class SampleClass1<T> : IBaseInterface1<T> { }          //No error
class SampleClass2<T> : IBaseInterface2<T, string> { }  //No error
```
### 泛型委托
>委托可以定义它自己的类型参数。引用泛型委托的代码可以指定类型参数以创建封闭式构造类型，就像实例化泛型类或调用泛型方法一样

```csharp
public delegate void Del<T>(T item);
public static void Notify(int i) { }

Del<int> m1 = new Del<int>(Notify);
```
## <span style="color:#0366d6;">方法组转换</span>
>它允许我们简单的为委托指定方法名称，而不需要使用关键字new或者显示调用委托的构造函数
```csharp
Del<int> m2 = Notify;
```
>根据典型设计模式定义事件时，泛型委托特别有用，因为发件人参数可以为强类型，无需在它和 Object 之间强制转换。
```csharp
delegate void StackEventHandler<T, U>(T sender, U eventArgs);

class Stack<T>
{
    public class StackEventArgs : System.EventArgs { }
    public event StackEventHandler<Stack<T>, StackEventArgs> stackEvent;

    protected virtual void OnStackChanged(StackEventArgs a)
    {
        stackEvent(this, a);
    }
}

class SampleClass
{
    public void HandleStackChange<T>(Stack<T> stack, Stack<T>.StackEventArgs args) { }
}

public static void Test()
{
    Stack<double> s = new Stack<double>();
    SampleClass o = new SampleClass();
    s.stackEvent += o.HandleStackChange;
}
```
### 泛型方法
>如泛型类中的泛型方法

### 泛型结构
>类似泛型类

### 泛型和特性
>仅允许自定义属性引用开放式泛型类型（即未向其提供任何类型参数的泛型类型）和封闭式构造泛型类型（即向所有类型参数提供参数的泛型类型）
```csharp
class CustomAttribute : System.Attribute
{
    public System.Object info;
}
```
>属性可引用开放式泛型类型
```csharp
public class GenericClass1<T> { }

[CustomAttribute(info = typeof(GenericClass1<>))]
class ClassA { }
```
>通过使用适当数量的逗号指定多个类型参数
```csharp
public class GenericClass2<T, U> { }

[CustomAttribute(info = typeof(GenericClass2<,>))]
class ClassB { }
```
>属性可引用封闭式构造泛型类型
```csharp
public class GenericClass3<T, U, V> { }

[CustomAttribute(info = typeof(GenericClass3<int, double, string>))]
class ClassC { }
```
>引用泛型类型参数的属性会导致编译时错误
```csharp
//[CustomAttribute(info = typeof(GenericClass3<int, T, string>))]  //Error
class ClassD<T> { }
```
>不能从Attribute继承泛型类型
```csharp
//public class CustomAtt<T> : System.Attribute {}  //Error
```
### 泛型约束
>约束告知编译器类型参数必须具备的功能
<table style="color:#0065b3;width:100%;border:0px;" >
<tr>
<td style="width:15%;border-left:0px;border-right:0px;color:black;font-weight:bold;">约束</td>
<td style="width:85%;border-left:0px;border-right:0px;color:black;font-weight:bold;">描述</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">where T : struct</td>
<td style="width:85%;border-left:0px;border-right:0px;">类型参数必须是不可为 null 的值类型。 有关可为 null 的值类型的信息，请参阅可为 null 的值类型。 由于所有值类型都具有可访问的无参数构造函数，因此 struct 约束表示 new() 约束，并且不能与 new() 约束结合使用。 此外，struct 约束也不能与 unmanaged 约束结合使用。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">where T : class</td>
<td style="width:85%;border-left:0px;border-right:0px;">类型参数必须是引用类型。 此约束还应用于任何类、接口、委托或数组类型。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">where T : notnull</td>
<td style="width:85%;border-left:0px;border-right:0px;">类型参数必须是不可为 null 的类型。 参数可以是 C# 8.0 或更高版本中的不可为 null 的引用类型，也可以是不可为 null 的值类型。 此约束还应用于任何类、接口、委托或数组类型。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">where T : unmanaged</td>
<td style="width:85%;border-left:0px;border-right:0px;">类型参数必须是不可为 null 的非托管类型。 unmanaged 约束表示 struct 约束，且不能与 struct 约束或 new() 约束结合使用。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">where T : new()</td>
<td style="width:85%;border-left:0px;border-right:0px;">类型参数必须具有公共无参数构造函数。 与其他约束一起使用时，new() 约束必须最后指定。 new() 约束不能与 struct 和 unmanaged 约束结合使用。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">where T : <基类名></td>
<td style="width:85%;border-left:0px;border-right:0px;">类型参数必须是指定的基类或派生自指定的基类。
</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">where T : <接口名称></td>
<td style="width:85%;border-left:0px;border-right:0px;">类型参数必须是指定的接口或实现指定的接口。 可指定多个接口约束。 约束接口也可以是泛型。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">where T : U</td>
<td style="width:85%;border-left:0px;border-right:0px;">为 T 提供的类型参数必须是为 U 提供的参数或派生自为 U 提供的参数。</td>
</tr>
</table>





