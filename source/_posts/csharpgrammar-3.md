---
title: C#1.0语法
date: 2020-02-20
tags: [程序设计语言，C#]
categories: csharp程序设计语言
---
C#1.0语法（发布于2000年6月,.NET Framework 1.0）
<!-- more -->
参考：<https://docs.microsoft.com/zh-cn/dotnet/csharp/>
# <span style="color:#0366d6;">C# 1.0</span>
## <span style="color:#0366d6;">数据类型</span>

<details>
<summary>展开查看</summary>

* 值类型
  * 简单类型
    * 有符号的整型：sbyte、short、int、long
    * 无符号的整型：byte、ushort、uint、ulong
    * Unicode 字符：char
    * IEEE 二进制浮点：float、double
    * 高精度十进制浮点数：decimal
    * 布尔：bool
  * 枚举类型
    * 格式为 enum E {...} 的用户定义类型
  * 结构类型
    * 格式为 struct S {...} 的用户定义类型
  * 可以为 null 的值类型
    * 值为 null 的其他所有值类型的扩展
* 引用类型
  * 类类型
    * 其他所有类型的最终基类：object
    * Unicode 字符串：string
    * 格式为 class C {...} 的用户定义类型
  * 接口类型
    * 格式为 interface I {...} 的用户定义类型
* 数组类型
  * 一维和多维，例如 int[] 和 int[,]
* 委托类型
  * 格式为 delegate int D(...) 的用户定义类型
</details> 

## <span style="color:#0366d6;">语言元素</span>
<details>
<summary>展开查看</summary>

* 程序结构
  * 了解 C# 语言中的关键组织概念：程序、命名空间、类型、成员和程序集。
* 类型和变量
  * 了解 C# 语言中的值类型、引用类型和变量。
* 表达式
  * 表达式是在操作数和运算符的基础之上构造而成。 表达式生成的是值。
* 语句
  * 语句用于表示程序的操作。
* 类和对象
  * 类是最基本的 C# 类型。 对象是类实例。 类是使用成员生成的，此主题也对此进行了介绍。
* 结构
  * 与类不同，结构是属于值类型的数据结构。
* 数组
  * 数组是一种数据结构，其中包含许多通过计算索引访问的变量。
* 接口
  * 接口定义了可由类和结构实现的协定。 接口可以包含方法、属性、事件和索引器。 接口不提供所定义的成员的实现代码，仅指定必须由实现接口的类或结构提供的成员。
* 委托
  * 委托类型表示对具有特定参数列表和返回类型的方法的引用。 通过委托，可以将方法视为可分配给变量并可作为参数传递的实体。 委托类似于其他一些语言中的函数指针概念，但与函数指针不同的是，委托不仅面向对象，还类型安全。
* 特性
  * 使用特性，程序可以指定关于类型、成员和其他实体的附加声明性信息。
</details> 

## <span style="color:#0366d6;">访问修饰符</span>
<details>
<summary>展开查看</summary>

* public
  * 访问不受限
* protected
  * 只能访问此类或派生自此类的类
* internal
  * 访问限于当前程序集（.exe、.dll 等）
* protected internal
  * 访问限于包含类、派生自包含类的类或同一程序集中的类
* private
  * 只能访问此类
* private protected
  * 访问限于同一程序集中的包含类或派生自包含类的类
</details> 

## <span style="color:#0366d6;">namespace</span>
>namespace 关键字用于声明包含一组相关对象的作用域。 可以使用命名空间来组织代码元素并创建全局唯一类型。
可包含：另一个命名空间，class，interface，struct，enum，delegate
## <span style="color:#0366d6;">using</span>
>using 语句定义一个范围，在此范围的末尾将释放对象
```csharp
using (var font1 = new Font("Arial", 10.0f)) 
{
    byte charset = font1.GdiCharSet;
}
```
>using允许在命名空间中使用类型，这样无需在该命名空间中限定某个类型的使用
```csharp
using System.Text;
```
>允许访问类型的静态成员和嵌套类型，而无需限定使用类型名称进行访问
```csharp
using static System.Math;
```
>为命名空间或类型创建别名。 这称为 using 别名指令
```csharp
using Project = PC.MyCompany.Project;
```
## <span style="color:#0366d6;">方法相关</span>

### 方法声明
```csharp
//public 访问修饰符
//virtual 虚方法修饰符
//int 返回值类型
//Drive 方法名称
//int miles, int speed 形参列表
//{  return 1; } 方法体 
public virtual int Drive(int miles, int speed) {  return 1; }
```
### 无方法体
```csharp
public abstract double GetTopSpeed();
```
### 参数修饰符
```csharp
* params 指定此参数采用可变数量的参数。
  //params 关键字之后不允许有任何其他参数，并且在方法声明中只允许有一个
  public static void UseParams(params int[] list){}
```
```csharp
* ref 指定此参数由引用传递，可能由调用方法读取或写入。
  //修饰在值类型前
  //调用前必须赋值
  public void SampleMethod(ref int i) { }

  ```
<details>
<summary>ref 修饰在引用类型前</summary>

```csharp
class Product
{
    public Product(string name, int newID)
    {
        ItemName = name;
        ItemID = newID;
    }

    public string ItemName { get; set; }
    public int ItemID { get; set; }
}

private static void ChangeByReference(ref Product itemRef)
{
    // Change the address that is stored in the itemRef parameter.   
    itemRef = new Product("Stapler", 99999);

    // You can change the value of one of the properties of
    // itemRef. The change happens to item in Main as well.
    itemRef.ItemID = 12345;
}

private static void ModifyProductsByReference()
{
    // Declare an instance of Product and display its initial values.
    Product item = new Product("Fasteners", 54321);
    System.Console.WriteLine("Original values in Main.  Name: {0}, ID: {1}\n",
        item.ItemName, item.ItemID);

    // Pass the product instance to ChangeByReference.
    ChangeByReference(ref item);
    System.Console.WriteLine("Back in Main.  Name: {0}, ID: {1}\n",
        item.ItemName, item.ItemID);
}

// This method displays the following output:
// Original values in Main.  Name: Fasteners, ID: 54321
// Back in Main.  Name: Stapler, ID: 12345 

```
</details>   

```csharp
* out 指定此参数由引用传递，由调用方法写入。
//调用返回前必须赋值
public void SampleMethod(out int i) {i=0; }
```

### 方法重载
>方法重载指的就是方法名称相同，但是参数不同
签名指的是方法名和参数列表
```csharp
public static int AddNumber(int num1,int num2)
{
    return num1 + num2;
}
       
public static double AddNumber(int num1, int num2,int num3)
{
    return num1 + num2;
}

public static double AddNumber(double num1, int num2)
{
    return num1 + num2;
}

```
### 方法重写
>重写基方法必须具有与 override 方法相同的签名,override 声明不能更改 virtual 方法的可访问性。 override 方法和 virtual 方法必须具有相同级别访问修饰符。override可以扩展或修改继承的<span style="color:#0065b3;">方法、属性、索引器或事件</span>的抽象或虚拟实现

```csharp
public class BaseC
{
    public int x;
    public virtual void Invoke() { }
}
public class DerivedC : BaseC
{
    override public void Invoke() { }
}
```
### 覆盖方法
>在用作声明修饰符时，new 关键字可以显式隐藏从基类继承的成员。 隐藏继承的成员时，该成员的派生版本将替换基类版本。 虽然可以不使用 new 修饰符来隐藏成员，但将收到编译器警告。 如果使用 new 来显式隐藏成员，将禁止此警告。
```csharp
public class BaseC
{
    public int x;
    public virtual void Invoke() { }
}
public class DerivedC : BaseC
{
    new public void Invoke() { }
}
```
## <span style="color:#0366d6;">类</span>
类的成员有：<span style="color:#0366d6;">字段，常量，属性，方法，事件，运算符，索引器，构造函数，终结器，嵌套类型</span>
```csharp
    //委托
    public delegate void MyEventHandler();
```

<details>
<summary>展开查看</summary>

```csharp
    public class Customer
    {
        //构造函数
        public Customer()
        {
            SomeEvent += Handler;
        }       

        //终结器
        ~Customer()
        {
            SomeEvent -= Handler;
        }

        //字段
        private string username;

        //数组
        string[] days = { "Sun", "Mon", "Tues", "Wed", "Thurs", "Fri", "Sat" };

        //事件
        public event MyEventHandler SomeEvent;

        //常量
        public const string COUNTRYNAME = "china";

        //属性
        public string UserName
        {
            get { return username; }
            set { username = value; }
        }

        //方法
        public void Handler()
        {

        }

        //方法
        private int GetDay(string testDay)
        {

            for (int j = 0; j < days.Length; j++)
            {
                if (days[j] == testDay)
                {
                    return j;
                }
            }

            throw new System.ArgumentOutOfRangeException(testDay, "testDay must be in the form \"Sun\", \"Mon\", etc");
        }
        
        //方法
        public void OnSomeEvent()
        {
            if (SomeEvent != null)
            {
                SomeEvent();
            }
        }

        //索引
        public int this[string day]
        {
            get
            {
                return (GetDay(day));
            }
        }
    }
```
</details>

### 创建对象
```csharp
Customer object1 = new Customer();
```
### 类继承
```csharp
public class Manager : Employee
{
    // Employee fields, properties, methods and events are inherited
    // New Manager fields, properties, methods and events go here...
}
```
### 抽象类
```csharp
//至少包含一个抽象方法
abstract class Motorcycle
{
   // Anyone can call this.
   public void StartEngine() {/* Method statements here */ }

   // Only derived classes can call this.
   protected void AddGas(int gallons) { /* Method statements here */ }

   // Derived classes can override the base class implementation.
   public virtual int Drive(int miles, int speed) { /* Method statements here */ return 1; }

   // Derived classes can override the base class implementation.
   public virtual int Drive(TimeSpan time, int speed) { /* Method statements here */ return 0; }

   // Derived classes must implement this.
   public abstract double GetTopSpeed(); 
}
```

```csharp
public class Constant: Expression
{
    double value;
    public Constant(double value) 
    {
        this.value = value;
    }
    public override double Evaluate(Dictionary<string,object> vars) 
    {
        return value;
    }
}
```
## <span style="color:#0366d6;">结构</span>
```csharp
public struct PostalAddress
{
    // Fields, properties, methods and events go here...
}
```
## <span style="color:#0366d6;">接口</span>
### 定义
```csharp
interface IEquatable<T>
{
    bool Equals(T obj);
}
```
### 实现
```csharp
interface IEquatable<T>
public class Car : IEquatable<Car>
{
    public string Make {get; set;}
    public string Model { get; set; }
    public string Year { get; set; }

    // Implementation of IEquatable<T> interface
    public bool Equals(Car car)
    {
        return this.Make == car.Make &&
               this.Model == car.Model &&
               this.Year == car.Year;
    }
}
```
## <span style="color:#0366d6;">委托</span>
>如类Customer中的委托

## <span style="color:#0366d6;">事件</span>
>如类Customer中的事件

## <span style="color:#0366d6;">特性</span>
### 使用
```csharp
[Serializable]
public class SampleClass
{
    // Objects of this type can be serialized.
}
```

### 创建
```csharp
[System.AttributeUsage(System.AttributeTargets.Class |  
                       System.AttributeTargets.Struct)  
]  
public class Author : System.Attribute  
{  
    private string name;  
    public double version;  
  
    public Author(string name)  
    {  
        this.name = name;  
        version = 1.0;  
    }  
} 
```
```csharp
[Author("P. Ackerman", version = 1.1)]  
class SampleClass  
{  
    // P. Ackerman's code goes here...  
}
```
## <span style="color:#0366d6;">C#1.2</span> 
>随 Visual Studio .NET 2003 一起提供的 C# 版本 1.2（.NET Framework 1.1）。 它对语言做了一些小改进。 最值得注意的是，从此版本开始，当 IEnumerator 实现 IDisposable 时，foreach 循环中生成的代码会在 IEnumerator 上调用 Dispose。

