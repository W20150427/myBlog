---
title: C#语法
date: 2020-02-19 12:00
tags: [程序设计语言，C#]
categories: csharp程序设计语言
---
<!-- more -->
## <span style="color:#0366d6;">C# 1.0
- 语句
- 类
- 结构
- 接口
- 委托
- 事件
- 特性
### 语句

>程序执行的操作采用语句表达。 常见操作包括声明变量、赋值、调用方法、循环访问集合，以及根据给定条件分支到一个或另一个代码块。 语句在程序中的执行顺序称为“控制流”或“执行流”。 根据程序对运行时所收到的输入的响应，在程序每次运行时控制流可能有所不同。
语句可以是以分号结尾的单行代码，也可以是语句块中的一系列单行语句。 语句块括在括号 {} 中，并且可以包含嵌套块。
<table style="color:#0065b3;width:100%;border:0px;" >
<tr>
<td style="width:15%;border-left:0px;border-right:0px;color:black;font-weight:bold;">类别</td>
<td style="width:85%;border-left:0px;border-right:0px;color:black;font-weight:bold;">C# 关键字/说明</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">声明语句</td>
<td style="width:85%;border-left:0px;border-right:0px;">声明语句引入新的变量或常量。 变量声明可以选择为变量赋值。 在常量声明中必须赋值。
</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">表达式语句</td>
<td style="width:85%;border-left:0px;border-right:0px;">用于计算值的表达式语句必须在变量中存储该值。 有关详细信息，请参阅表达式语句。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">选择语句</td>
<td style="width:85%;border-left:0px;border-right:0px;">选择语句用于根据一个或多个指定条件分支到不同的代码段。 有关详细信息，请参阅下列主题：if else switch case</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">迭代语句</td>
<td style="width:85%;border-left:0px;border-right:0px;">迭代语句用于遍历集合（如数组），或重复执行同一组语句直到满足指定的条件。 有关详细信息，请参阅下列主题：do for foreach in while</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">跳转语句</td>
<td style="width:85%;border-left:0px;border-right:0px;">跳转语句将控制转移给另一代码段。 有关详细信息，请参阅下列主题：break continue default goto return yield</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">异常处理语句</td>
<td style="width:85%;border-left:0px;border-right:0px;">异常处理语句用于从运行时发生的异常情况正常恢复。 有关详细信息，请参阅下列主题：throw try-catch try-finally try-catch-finally</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">Checked 和 unchecked</td>
<td style="width:85%;border-left:0px;border-right:0px;">Checked 和 unchecked 语句用于指定将结果存储在变量中、但该变量过小而不能容纳结果值时，是否允许数值运算导致溢出。 有关详细信息，请参阅 checked 和 unchecked。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">await 语句</td>
<td style="width:85%;border-left:0px;border-right:0px;">如果用 async 修饰符标记方法，则可以使用该方法中的 await 运算符。 在控制到达异步方法的 await 表达式时，控制将返回到调用方，该方法中的进程将挂起，直到等待的任务完成为止。 任务完成后，可以在方法中恢复执行。

有关简单示例，请参阅方法的“异步方法”一节。 有关详细信息，请参阅 async 和 await 的异步编程</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">yield return 语句</td>
<td style="width:85%;border-left:0px;border-right:0px;">迭代器对集合执行自定义迭代，如列表或数组。 迭代器使用 yield return 语句返回元素，每次返回一个。 到达 yield return 语句时，会记住当前在代码中的位置。 下次调用迭代器时，将从该位置重新开始执行。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">fixed 语句	</td>
<td style="width:85%;border-left:0px;border-right:0px;">fixed 语句禁止垃圾回收器重定位可移动的变量。 有关详细信息，请参阅 fixed。/td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">lock 语句</td>
<td style="width:85%;border-left:0px;border-right:0px;">lock 语句用于限制一次仅允许一个线程访问代码块。 有关详细信息，请参阅 lock。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">带标签的语句</td>
<td style="width:85%;border-left:0px;border-right:0px;">可以为语句指定一个标签，然后使用 goto 关键字跳转到该带标签的语句。 （参见下一行中的示例。）</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">空语句</td>
<td style="width:85%;border-left:0px;border-right:0px;">空语句只含一个分号。 不执行任何操作，可以在需要语句但不需要执行任何操作的地方使用。</td>
</tr>
</table>

### 类
```csharp
//[access modifier] - [class] - [identifier]
public class Customer
{
   // Fields, properties, methods and events go here...
}
```
#### 创建对象
```csharp
Customer object1 = new Customer();
```
#### 类继承
```csharp
public class Manager : Employee
{
    // Employee fields, properties, methods and events are inherited
    // New Manager fields, properties, methods and events go here...
}
```
### 结构
```csharp
public struct PostalAddress
{
    // Fields, properties, methods and events go here...
}
```
### 接口
#### 定义
```csharp
interface IEquatable<T>
{
    bool Equals(T obj);
}
```
#### 实现
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
### 委托

#### 定义
```csharp
// Define the delegate type:
public delegate int Comparison<in T>(T left, T right);
```
#### 调用
```csharp
// inside a class definition:

// Declare an instance of that type:
public Comparison<T> comparator;

int result = comparator(left, right);
```
### 事件
#### 定义
```csharp
public event EventHandler<FileListArgs> Progress;
```
#### 调用
```csharp
Progress?.Invoke(this, new FileListArgs(file));
```
### 特性

#### 使用
```csharp
[Serializable]
public class SampleClass
{
    // Objects of this type can be serialized.
}
```

#### 创建
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