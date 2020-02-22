---
title: C#拾遗
date: 2020-02-19
tags: [程序设计语言，C#]
categories: csharp程序设计语言
---
C#常见知识点整理
<!-- more -->
参考：<https://docs.microsoft.com/zh-cn/dotnet/csharp/>

## <span style="color:#0366d6;">类型的默认值</span>

<table style="color:#0065b3;width:100%;border:0px;" >
<tr>
<td style="width:15%;border-left:0px;border-right:0px;color:black;font-weight:bold;">类型</td>
<td style="width:85%;border-left:0px;border-right:0px;color:black;font-weight:bold;">默认值</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">任何引用类型</td>
<td style="width:85%;border-left:0px;border-right:0px;">null</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">任何内置整数数值类型</td>
<td style="width:85%;border-left:0px;border-right:0px;">0（零）</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">任何内置浮点型数值类型</td>
<td style="width:85%;border-left:0px;border-right:0px;">0（零）</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">bool</td>
<td style="width:85%;border-left:0px;border-right:0px;">false</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">char</td>
<td style="width:85%;border-left:0px;border-right:0px;">'\0' (U + 0000)</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">enum</td>
<td style="width:85%;border-left:0px;border-right:0px;">表达式 (E)0 生成的值，其中 E 是枚举标识符。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">struct/td>
<td style="width:85%;border-left:0px;border-right:0px;">通过如下设置生成的值：将所有值类型的字段设置为其默认值，将所有引用类型的字段设置为 null。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;border-right:0px;">任何可以为 null 的值类型</td>
<td style="width:85%;border-left:0px;border-right:0px;">HasValue 属性为 false 且 Value 属性未定义的实例。 该默认值也称为可以为 null 的值类型的“null” 值。</td>
</tr>
</table>

## <span style="color:#0366d6;">强制转换和类型转换</span>
### 隐式转换
>对于内置数值类型，如果要存储的值无需截断或四舍五入即可适应变量，则可以进行隐式转换。 对于整型类型，这意味着源类型的范围是目标类型范围的正确子集。例如，long 类型的变量（64 位整数）能够存储 int（32 位整数）可存储的任何值。 在下面的示例中，编译器先将右侧的 num 值隐式转换为 long 类型，再将它赋给 bigNum。
```csharp
// Implicit conversion. A long can
// hold any value an int can hold, and more!
int num = 2147483647;
long bigNum = num;
```
### 强制转换
>但是，如果进行转换可能会导致信息丢失，则编译器会要求执行显式转换，显式转换也称为强制转换。强制转换是显式告知编译器你打算进行转换且你知道可能会发生数据丢失的一种方式。若要执行强制转换，请在要转换的值或变量前面的括号中指定要强制转换到的类型。下面的程序将 double 强制转换为 int。如不强制转换则该程序不会进行编译。
```csharp
 double x = 1234.7;
 int a;
 // Cast double to int.
 a = (int)x;
```
### 用户定义转换运算符
>operator和implicit(隐式)或explicit(显示)关键字分别用于定义隐式转换或显式转换。
<details>
<summary>展开查看</summary>

```csharp
using System;

public readonly struct Digit
{
    private readonly byte digit;

    public Digit(byte digit)
    {
        if (digit > 9)
        {
            throw new ArgumentOutOfRangeException(nameof(digit), "Digit cannot be greater than nine.");
        }
        this.digit = digit;
    }

    public static implicit operator byte(Digit d) => d.digit;
    public static explicit operator Digit(byte b) => new Digit(b);

    public override string ToString() => $"{digit}";
}

public static class UserDefinedConversions
{
    public static void Main()
    {
        var d = new Digit(7);
        
        byte number = d;
        Console.WriteLine(number);  // output: 7

        Digit digit = (Digit)number;
        Console.WriteLine(digit);  // output: 7
    }
}
```
</details>

### 使用帮助程序类进行转换

```csharp
 System.BitConverter,System.Convert,Int32.Parse
 ```

 ## <span style="color:#0366d6;">装箱和取消装箱</span>

 ### 装箱
 >装箱是值类型到 object 类型或到此值类型所实现的任何接口类型的隐式转换。对值类型装箱会在堆中分配一个对象实例，并将该值复制到新的对象中。
 ```csharp
 int i = 123;
 // Boxing copies the value of i into object o.
object o = i; 
 ```
 ### 拆箱
 >取消装箱是从 object 类型到值类型或从接口类型到实现该接口的值类型的显式转换。取消装箱操作包括：
1.检查对象实例，以确保它是给定值类型的装箱值。
2.将该值从实例复制到值类型变量中。
```csharp
int i = 123;      // a value type
object o = i;     // boxing
int j = (int)o;   // unboxing
```
