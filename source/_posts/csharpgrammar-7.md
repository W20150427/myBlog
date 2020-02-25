---
title: C#5.0语法
date: 2020-02-26
tags: [程序设计语言，C#]
categories: csharp程序设计语言
---
C#5.0语法（发布于 2012，.NET Framework 4.5）
对此版本中所做的几乎所有工作都归入另一个突破性语言概念：适用于异步编程的 async 和 await 模型。
<!-- more -->
参考：<https://docs.microsoft.com/zh-cn/dotnet/csharp/>
# <span style="color:#0366d6;">C#5.0语法</span>
## <span style="color:#0366d6;">调用方信息</span>
>通过使用调用方信息特性，可获取有关方法的调用方的信息。 可以获取源代码的文件路径、源代码中的行号和调用方的成员名称。 此信息有助于跟踪、调试和创建诊断工具。

<table style="color:#0065b3;width:100%;border:0px;" >
<tr>
<td style="width:30%;border-left:0px;border-right:0px;color:black;font-weight:bold;">特性</td>
<td style="width:60%;border-left:0px;border-right:0px;color:black;font-weight:bold;">说明</td>
<td style="width:10%;border-left:0px;border-right:0px;color:black;font-weight:bold;">类型</td>
</tr>
<tr>
<td style="width:30%;border-left:0px;border-right:0px;">CallerFilePathAttribute</td>
<td style="width:60%;border-left:0px;border-right:0px;">包含调用方的源文件的完整路径。这是编译时的文件路径。</td>
<td style="width:10%;border-left:0px;border-right:0px;">String</td>
</tr>
<tr>
<td style="width:30%;border-left:0px;border-right:0px;">CallerLineNumberAttribute</td>
<td style="width:60%;border-left:0px;border-right:0px;">源文件中调用方法的行号。</td>
<td style="width:10%;border-left:0px;border-right:0px;">Integer</td>
</tr>
<tr>
<td style="width:30%;border-left:0px;border-right:0px;">CallerMemberNameAttribute</td>
<td style="width:60%;border-left:0px;border-right:0px;">调用方的方法或属性名称。 请参阅本主题后面的成员名称。</td>
<td style="width:10%;border-left:0px;border-right:0px;">String</td>
</tr>
</table>

```csharp
public void DoProcessing()
{
    TraceMessage("Something happened.");
}

public void TraceMessage(string message,
        [System.Runtime.CompilerServices.CallerMemberName] string memberName = "",
        [System.Runtime.CompilerServices.CallerFilePath] string sourceFilePath = "",
        [System.Runtime.CompilerServices.CallerLineNumber] int sourceLineNumber = 0)
{
    System.Diagnostics.Trace.WriteLine("message: " + message);
    System.Diagnostics.Trace.WriteLine("member name: " + memberName);
    System.Diagnostics.Trace.WriteLine("source file path: " + sourceFilePath);
    System.Diagnostics.Trace.WriteLine("source line number: " + sourceLineNumber);
}

// Sample Output:
//  message: Something happened.
//  member name: DoProcessing
//  source file path: c:\Visual Studio Projects\CallerInfoCS\CallerInfoCS\Form1.cs
//  source line number: 31
```
>CallerMemberName 特性时返回的成员名称
<table style="color:#0065b3;width:100%;border:0px;" >
<tr>
<td style="width:30%;border-left:0px;border-right:0px;color:black;font-weight:bold;">调用发生中</td>
<td style="width:70%;border-left:0px;border-right:0px;color:black;font-weight:bold;">成员名称结果</td>
</tr>
<tr>
<td style="width:30%;border-left:0px;border-right:0px;">方法、属性或事件</td>
<td style="width:70%;border-left:0px;border-right:0px;">从中发起调用的方法、属性或事件的名称。</td>
</tr>
<tr>
<td style="width:30%;border-left:0px;border-right:0px;">构造函数</td>
<td style="width:70%;border-left:0px;border-right:0px;">字符串“.ctor”</td>
</tr>
<tr>
<td style="width:30%;border-left:0px;border-right:0px;">静态构造函数</td>
<td style="width:70%;border-left:0px;border-right:0px;">字符串“.cctor”</td>
</tr>
<tr>
<td style="width:30%;border-left:0px;border-right:0px;">析构函数</td>
<td style="width:70%;border-left:0px;border-right:0px;">字符串“Finalize”</td>
</tr>
<tr>
<td style="width:30%;border-left:0px;border-right:0px;">用户定义的运算符或转换</td>
<td style="width:70%;border-left:0px;border-right:0px;">为成员生成的名称，例如，“op_Addition”。</td>
</tr>
<tr>
<td style="width:30%;border-left:0px;border-right:0px;">特性构造函数</td>
<td style="width:70%;border-left:0px;border-right:0px;">要应用特性的方法或属性的名称。 如果该特性是成员中的任何元素（如参数、返回值或泛型参数），则此结果是与该元素关联的成员的名称。</td>
</tr>
<tr>
<td style="width:30%;border-left:0px;border-right:0px;">无包含的成员（例如，程序集级别或应用于类型的特性）</td>
<td style="width:70%;border-left:0px;border-right:0px;">可选参数的默认值。</td>
</tr>
</table>

## <span style="color:#0366d6;">异步编程</span>
>异步编程的核心是 Task 和 Task<T> 对象，这两个对象对异步操作建模。 它们受关键字 async 和 await 的支持。