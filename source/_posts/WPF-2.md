---
title: WPF Style 
date: 2020-04-19
tags: [WPF]
categories: WPF
---
<!-- more -->
# <span style="color:#0366d6;">Style 常见写法</span>
隐式写法,对所有Label有效
``` Html
 <Style TargetType="Label">
  <Setter Property="Padding" Value="0"/>
 </Style>
```
显示写法,对所有Label.Style=lblStyle有效
XAML语法 Style="{StaticResource lblStyle有效}"
C#语法Label.Style = Resources["lblStyle"] as Style;
``` Html
 <Style  x:Key="lblStyle" TargetType="Label">
  <Setter Property="Padding" Value="0"/>
 </Style>
```
显示写法,类似隐式写法
``` Html
 <Style x:Key="{x:Type Label}" TargetType="Label">
  <Setter Property="Padding" Value="0"/>
 </Style>
```

内部写法，类似于显示写法，对应用Key才有效，只是语法不同
``` Html
<Style x:Key="lblStyle">
  <Setter Property="Control.Padding" Value="0"/>
</Style>
```
# <span style="color:#0366d6;">Style 继承</span>
继承借助关键字BasedOn
``` Html
<Style x:Key="myLabelStyle" TargetType="Label" BasedOn="{StaticResource lblStyle}">
  <Setter Property="Width" Value="10"/>
</Style>
```

# <span style="color:#0366d6;">Style 触发器</span>
属性触发器
``` Html
 <Style  x:Key="lblStyle" TargetType="Label">
 <DataTrigger Binding="{Binding Path=IsTaped}" Value="true">
  <Setter Property="Background" Value="Red"/>
 </DataTrigger>
 </Style>
```
可以是事件属性
``` Html
 <Style  x:Key="lblStyle" TargetType="Label">
  <Style.Triggers>
    <Trigger Property="IsMouseOver" Value="True">
      <Setter Property="Background" Value="#3f48cc"/>                 
    </Trigger>
  </Style.Triggers>
 </Style>
```
多条件触发器
``` Html
<Style>
 <Style.Triggers>
<MultiDataTrigger>
    <MultiDataTrigger.Conditions>
        <Condition Binding="{Binding Path=IsTaped}" Value="true"/>
         <Condition Binding="{Binding Path=Name}" Value="bin"/>
    </MultiDataTrigger.Conditions>
    <MultiDataTrigger.Setters>
        <Setter  Property="Background" Value="Blue"></Setter>
    </MultiDataTrigger.Setters>
</MultiDataTrigger>              
</Style.Triggers>
</Style>
```
``` Html
<Style>
 <Style.Triggers>
<MultiTrigger>
    <MultiTrigger.Conditions>
        <Condition Binding="{Binding Path=IsTaped}" Value="true"/>
         <Condition Binding="{Binding Path=Name}" Value="bin"/>
    </MultiTrigger.Conditions>
    <MultiTrigger.Setters>
        <Setter  Property="Background" Value="Blue"></Setter>
    </MultiTrigger.Setters>
</MultiTrigger>              
</Style.Triggers>
</Style>
```
事件触发器
``` Html
 <Style  x:Key="lblStyle" TargetType="Label">
  <Style.Triggers>
   <EventSetter Event="Label.IsMouseOver" Handler="element_MouseOver"/>   
  </Style.Triggers>
 </Style>
```
# <span style="color:#0366d6;">样式注意点</span>
>动态改变属性,可以调用Window,UserControl,Application,Page的 Resources.MergedDictionaries来添加style
>保持默认样式，Style="{x:Null}"
