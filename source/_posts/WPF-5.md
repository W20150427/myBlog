---
title: WPF 依赖属性
date: 2020-07-26
tags: [WPF]
categories: WPF
---
<!-- more -->
参考:WPF 编程宝典
https://docs.microsoft.com/zh-cn/dotnet/framework/wpf/advanced/properties-wpf
https://www.cnblogs.com/Zhouyongh/archive/2009/09/10/1564099.html
>依赖属性的出现，主要是为了解决对象的属性带来的膨胀，因为依赖属性设计为静态，
所有对象共用一份，减少消耗，由于其内部的设计，每个对象又可以独立设置和拥有依赖属性的值
其原理参考引用链接中的文章
# <span style="color:#0366d6;">模拟依赖属性实现</span>
>引用自https://www.cnblogs.com/Zhouyongh/archive/2009/09/10/1564099.html
<details>
<summary>点开查看</summary>

```csharp
static void Main(string[] args)
{
    DependencyObject obj = new DependencyObject();
    SubDependencyObject subObj = new SubDependencyObject();
    string sd = subObj.Name;
}

public class DependencyObject
{
    private List<EffectiveValueEntry> _effectiveValues = new List<EffectiveValueEntry>();

    public static readonly DependencyProperty NameProperty = DependencyProperty.Register("Name", typeof(string), typeof(DependencyObject), "Name");

    public object GetValue(DependencyProperty dp)
    {
        EffectiveValueEntry effectiveValue = _effectiveValues.FirstOrDefault((i) => i.PropertyIndex == dp.Index);
        if (effectiveValue.PropertyIndex != 0)
        {
            return effectiveValue.Value;
        }
        else
        {
            PropertyMetadata metadata;
            metadata = DependencyProperty.RegisteredDps[dp.HashCode].GetMetadata(this.GetType());
            return metadata.Value;
        }
    }

    public void SetValue(DependencyProperty dp, object value)
    {
        EffectiveValueEntry effectiveValue = _effectiveValues.FirstOrDefault((i) => i.PropertyIndex == dp.Index);
        if (effectiveValue.PropertyIndex != 0)
        {
            effectiveValue.Value = value;
        }
        else
        {
            effectiveValue = new EffectiveValueEntry() { PropertyIndex = dp.Index, Value = value };
            _effectiveValues.Add(effectiveValue);
        }
    }

    public string Name
    {
        get
        {
            return (string)GetValue(NameProperty);
        }
        set
        {
            SetValue(NameProperty, value);
        }
    }
}

public class SubDependencyObject : DependencyObject
{
    static SubDependencyObject()
    {
        NameProperty.OverrideMetadata(typeof(SubDependencyObject), new PropertyMetadata("SubName"));
    }
}

public class DependencyProperty
{
    private static int globalIndex = 0;
    internal static Dictionary<object, DependencyProperty> RegisteredDps = new Dictionary<object, DependencyProperty>();
    internal string Name;
    internal object Value;
    internal int Index;
    internal object HashCode;
    private PropertyMetadata _defaultMetadata;
    private List<PropertyMetadata> _metadataMap = new List<PropertyMetadata>();


    private DependencyProperty(string name, Type propertyName, Type ownerType, object defaultValue)
    {
        this.Name = name;
        this.Value = defaultValue;
        this.HashCode = name.GetHashCode() ^ ownerType.GetHashCode();

        PropertyMetadata metadata = new PropertyMetadata(defaultValue) { Type = ownerType };
        _metadataMap.Add(metadata);
        _defaultMetadata = metadata;
    }

    public static DependencyProperty Register(string name, Type propertyType, Type ownerType, object defaultValue)
    {
        DependencyProperty dp = new DependencyProperty(name, propertyType, ownerType, defaultValue);
        globalIndex++;
        dp.Index = globalIndex;
        RegisteredDps.Add(dp.HashCode, dp);
        return dp;
    }

    public void OverrideMetadata(Type forType, PropertyMetadata metadata)
    {
        metadata.Type = forType;
        _metadataMap.Add(metadata);
    }

    public PropertyMetadata GetMetadata(Type type)
    {
        PropertyMetadata medatata = _metadataMap.FirstOrDefault((i) => i.Type == type) ??
            _metadataMap.FirstOrDefault((i) => type.IsSubclassOf(i.Type));
        if (medatata == null)
        {
            medatata = _defaultMetadata;
        }
        return medatata;
    }
}

internal struct EffectiveValueEntry
{
    internal int PropertyIndex { get; set; }

    internal object Value { get; set; }
}

public class PropertyMetadata
{
    public Type Type { get; set; }
    public object Value { get; set; }

    public PropertyMetadata(object defaultValue)
    {
        this.Value = defaultValue;
    }
}
```
</details>

# <span style="color:#0366d6;">依赖属性优先级</span>
## <span style="color:#0366d6;">优先级</span>
>基值：3-11都称为基值，但属性系统强制的优先级最高，在CoerceValueCallback时发生
本地值：xaml里面直接给属性赋值或者C#代码中直接赋值，```xml <Label x:Name="lblTest"  Width="100"/>```或者lblTest.Width=100
当前值或者叫做有效值（EffctiveValue）：DependencyObject提供了GetValue方法来取得属性值，是最终对外的那个值
>1. 属性系统强制
2. 活动动画或具有 Hold 行为的动画
3. 本地值
4. TemplatedParent 模板属性
5. 隐式样式
6. 样式触发器
7. 模板触发器
8. 样式资源库
9. 默认（主题）样式
   - 主题样式中的活动触发器
   - 主题样式中的资源库
10. 继承
11. 来自依赖属性元数据的默认值

## <span style="color:#0366d6;">使用过程</span>
![过程](https://picb.zhimg.com/80/v2-4cc97c6644074daf299805cddf4463fb_720w.png)
>1. 第一步，确定Base Value，对同一个属性的赋值可能发生在很多地方。比如控件的背景（Background），可能在Style或者控件的构造函数中都对它进行了赋值，这个Base Value就要确定这些值中优先级最高的值，把它作为Base Value。
2. 第二步，估值。如果从第一步得到的值是一个表达式值（Expression），比如说一个绑定，WPF属性系统需要把它转化成一个实际值。
3. 第三步，动画。如果当前属性正在作动画，那么因动画而产生的值会优于前面获得的值，这个也就是WPF中常说的动画优先。
4. 第四步，强制。如果我们在FrameworkPropertyMetadata中传入了CoerceValueCallback，WPF属性系统会回调我们传入的的delagate，进行数据的强制赋值。在属性赋值过程中，Coerce拥有最高的优先级，这个优先级要大于动画的优先级别。
5. 第五步，验证。如果在Register的时候传入了ValidateValueCallback，那么最后WPF会调用我们传入的delegate，来验证数据的有效性。当数据无效时会抛出异常来通知.

# <span style="color:#0366d6;">依赖项属性元数据</span>
>依赖项属性的元数据作为一个对象存在，可以通过设置该属性，来实现依赖属性的某些功能
1. 依赖项属性的默认值
2. 对影响每个所有者类型的强制行（强制回调）为或更改通知行为（属性更改通知）的回调实现的引用。 请注意，这些回调通常是用非公共访问级别定义的，因此，除非实际引用位于您允许的访问范围内，否则通常无法从元数据获得这些引用
3. 框架性元属性
4. 将类作为现有依赖项属性的所有者来添加,AddOwner（添加共享依赖属性）。重写元数据,OverrideMetadata。


# <span style="color:#0366d6;">依赖项属性验证和强制</span>
>验证回调：属性系统可在多种不同操作中使用回调。 这包括按默认值初始类型初始化、通过调用SetValue进行编程更改或尝试使用提供的新默认值覆盖元数据。 如果验证回调是通过其中任何一种操作调用的，并且返回 false，则会引发异常。
```csharp
private static bool IsValidReading(object value){}
```
>当前值的属性更改回叫用于将更改转发到其他依赖属性
```csharp
private  static void OnCurrentReadingChanged(DependencyObject d, DependencyPropertyChangedEventArgs e){}
```
>强制值回叫会检查当前属性可能依赖的属性的值，并在必要时强制当前值
```csharp
private static object CoerceCurrentReading(DependencyObject d, object value){}
```
<details>
<summary>点开查看</summary>

```csharp
public class Gauge : DependencyObject
{
public static readonly DependencyProperty CurrentReadingProperty = DependencyProperty.Register(
"CurrentReading",
typeof(double),
typeof(Gauge),
new FrameworkPropertyMetadata(
double.NaN,
FrameworkPropertyMetadataOptions.AffectsMeasure,
new PropertyChangedCallback(PropertyChangedCallback),
new CoerceValueCallback(CoerceCurrentReading)),
new ValidateValueCallback(IsValidReading));

public Gauge(int i)
{
Console.WriteLine(i);
}

private static void PropertyChangedCallback(DependencyObject d, DependencyPropertyChangedEventArgs e)
{

}

public double CurrentReading
{
get { return (double)GetValue(CurrentReadingProperty); }
set { SetValue(CurrentReadingProperty, value); }
}

private static bool IsValidReading(object value)
{
Console.WriteLine("IsValidReading");
Double v = (Double)value;
return (!v.Equals(Double.NegativeInfinity) && !v.Equals(Double.PositiveInfinity));
}

private static object CoerceCurrentReading(DependencyObject d, object baseValue)
{
return 1.2;
}
}
```
</details>

# <span style="color:#0366d6;">附加属性</span>
>附加属性旨在用作可在任何对象上设置的一类全局属性。在WPF中，附加属性通常定义为没有常规属性“包装”的一种特殊形式的依赖项属性。
一个示例是 DockPanel.Dock 属性。 DockPanel.Dock 属性创建为附加属性，因为它将在 DockPanel 中包含的元素上设置，而不是在 DockPanel 本身设置

>自定义附属属性
```xml
<Grid>
<Canvas>
<TextBlock Text="Hello World" FontSize="20" local:Bruce.Top="400"></TextBlock>
</Canvas>
</Grid>
```
```csharp
public class Bruce:DependencyObject
{
public static double GetLeft(DependencyObject obj)
{
return (double)obj.GetValue(LeftProperty);
}
public static void SetLeft(DependencyObject obj,object value)
{
obj.SetValue(LeftProperty, value);
}
public static DependencyProperty
LeftProperty = DependencyProperty.RegisterAttached
("Left", typeof(double), typeof(Bruce), new PropertyMetadata(0.0,(obj,e)=> 
{
var element=obj as FrameworkElement;//目标控件
if (element.Parent.GetType() == typeof(Canvas))
{
element.Margin = new Thickness((double)e.NewValue,0,0,0);
}
}));
}
```






