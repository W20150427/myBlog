---
title: WPF 布局 基础篇
date: 2020-07-19
tags: [WPF]
categories: WPF
---
<!-- more -->
参考:WPF 编程宝典
# <span style="color:#0366d6;">布局元素</span>
<table style="color:#0065b3;width:100%;border:0px;" >
<tr>
<td style="width:15%;border-left:0px;border-right:0px;color:black;font-weight:bold;">名称</td>
<td style="width:85%;border-left:0px;border-right:0px;color:black;font-weight:bold;">说明</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;color:#0065b3;border-right:0px;">StackPanel</td>
<td style="width:85%;border-left:0px;color:black;border-right:0px;">在水平或者垂直的栈中堆放元素</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;color:#0065b3;border-right:0px;">WrapPanel</td>
<td style="width:85%;border-left:0px;color:black;border-right:0px;">按从左到右的顺序位置定位子元素，在包含框的边缘处将内容切换到下一行。 后续排序按照从上至下或从右至左的顺序进行，具体取决于 Orientation 属性的值。</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;color:#0065b3;border-right:0px;">DockPanel</td>
<td style="width:85%;border-left:0px;color:black;border-right:0px;">用来定位子内容的布局容器的边缘</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;color:#0065b3;border-right:0px;">Grid</td>
<td style="width:85%;border-left:0px;color:black;border-right:0px;">定义由列和行组成的灵活的网格区域</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;color:#0065b3;border-right:0px;">UniformGrid</td>
<td style="width:85%;border-left:0px;color:black;border-right:0px;">提供一种在网格(网格中的所有单元格都具有相同的大小)中排列内容的方法</td>
</tr>
<tr>
<td style="width:15%;border-left:0px;color:#0065b3;border-right:0px;">Canvas</td>
<td style="width:85%;border-left:0px;color:black;border-right:0px;">定义一个区域，可在其中使用相对于 Canvas 区域的坐标以显式方式来定位子元素</td>
</tr>
</table>

## <span style="color:#0366d6;">StackPanel</span>
>该面板简单的在单行或者单列以堆栈形式放置其子元素，通过 Orientation="Horizontal"或者"Vertical"
```xml
<StackPanel Margin="3" Name="stackPanel1" >
    <Label Margin="3" HorizontalAlignment="Center">
      A Button Stack
    </Label>
    <Button Margin="3" MaxWidth="200" MinWidth="100">Button 1</Button>
    <Button Margin="3" MaxWidth="200" MinWidth="100">Button 2</Button>
    <Button Margin="3" MaxWidth="200" MinWidth="100">Button 3</Button>
    <Button Margin="3" MaxWidth="200" MinWidth="100">Button 4</Button>
    <CheckBox Name="chkVertical" Margin="10" HorizontalAlignment="Center"
     Checked="chkVertical_Checked" Unchecked="chkVertical_Unchecked">
      Use Vertical Orientation</CheckBox>            
</StackPanel>
```
## <span style="color:#0366d6;">WrapPanel</span>
>该面板在可能的空间里面，以一次一行或者一列的方式布置控件。默认情况下Orientation="Horizontal"
```xml
<WrapPanel Margin="3">
    <Button VerticalAlignment="Top">Top Button</Button>
    <Button MinHeight="60">Tall Button 2</Button>
    <Button VerticalAlignment="Bottom">Bottom Button</Button>
    <Button>Stretch Button</Button>
    <Button VerticalAlignment="Center">Centered Button</Button>   
</WrapPanel>
```
## <span style="color:#0366d6;">DockPanel</span>
>DockPanel面吧是更有趣的布局选项。它沿着一条外边缘来拉伸所包含的控件。理解该面板最简便的方式是，考虑一下位于许多Windows应用程序窗口顶部的工具栏，>这些工具栏停靠到窗口顶部。与StackPanel面板类似，被停靠的元素选择它们的布局的一方面。例如，如果将一个按钮停靠在DockPanel面板顶部，该按钮被拉伸至>DockPanel面板的整个宽度，但根据内容和MinHeight属性为其设置所需的高度。而如果将一个按钮停靠到容器左边，该按钮的高度将被拉伸以适应容器的高度，而其>宽度可以根据需要自由添加。
```xml
<DockPanel LastChildFill="True">
      <Button DockPanel.Dock="Top">A Stretched Top Button</Button>
      <Button DockPanel.Dock="Top" HorizontalAlignment="Center">A Centered Top Button</Button>
      <Button DockPanel.Dock="Top" HorizontalAlignment="Left">A Left-Aligned Top Button</Button>
      <Button DockPanel.Dock="Bottom">Bottom Button</Button>
      <Button DockPanel.Dock="Left">Left Button</Button>
      <Button DockPanel.Dock="Right">Right Button</Button>
      <Button >Remaining Space</Button>
</DockPanel>
```
## <span style="color:#0366d6;">Grid</span>
>Grid面板将元素分割到不可见的单元格中
```xml
<Grid ShowGridLines="True">
  <Grid.RowDefinitions>
  <RowDefinition />
  <RowDefinition />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
  <ColumnDefinition />
  <ColumnDefinition />
  <ColumnDefinition />
  </Grid.ColumnDefinitions>
  <Button Grid.Row="0" Grid.Column="0">Top Left</Button>
  <Button Grid.Row="0" Grid.Column="1">Middle Left</Button>
  <Button Grid.Row="1" Grid.Column="2">Bottom Right</Button>
  <Button Grid.Row="1" Grid.Column="1">Bottom Middle</Button>
</Grid>
```
>绝对设置尺寸
```xml
 <ColumnDefinition Width="50"></ColumnDefinition>
```
>自动设置尺寸
```xml
 <ColumnDefinition Width="Auto"></ColumnDefinition>
```
>按比例设置尺寸
```xml
 <ColumnDefinition Width="*"></ColumnDefinition>
 <ColumnDefinition Width="2*"></ColumnDefinition>
```
>分割窗口
<details>
<summary>点开查看</summary>
```xml
<Grid>
  <Grid.RowDefinitions>
  <RowDefinition></RowDefinition>
  <RowDefinition></RowDefinition>
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
  <ColumnDefinition MinWidth="100"></ColumnDefinition>
  <ColumnDefinition Width="Auto"></ColumnDefinition>
  <ColumnDefinition MinWidth="50"></ColumnDefinition>
  </Grid.ColumnDefinitions>
  <Button Grid.Row="0" Grid.Column="0" Margin="3">Left</Button>
  <Button Grid.Row="0" Grid.Column="2" Margin="3">Right</Button>
  <Button Grid.Row="1" Grid.Column="0" Margin="3">Left</Button>
  <Button Grid.Row="1" Grid.Column="2" Margin="3">Right</Button>
  <GridSplitter Grid.Row="0" Grid.Column="1" Grid.RowSpan="2"                
                  Width="3" VerticalAlignment="Stretch" HorizontalAlignment="Center"
                  ShowsPreview="False"></GridSplitter>

</Grid>
```
</details>


>共享尺寸组
<details>
<summary>点开查看</summary>
```xml
<Grid Margin="3" Grid.IsSharedSizeScope="True">
<Grid.RowDefinitions>
    <RowDefinition />
    <RowDefinition Height="Auto" />
    <RowDefinition />
</Grid.RowDefinitions>
<Grid
    Grid.Row="0"
    Margin="3"
    Background="LightYellow"
    ShowGridLines="True">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" SharedSizeGroup="TextLabel" />
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>
    <Label Margin="5">A very long bit of text</Label>
    <!-- <GridSplitter Grid.Column="1" VerticalAlignment="Stretch" HorizontalAlignment="Center" Width="10"></GridSplitter> -->
    <Label Grid.Column="1" Margin="5">More text</Label>
    <TextBox Grid.Column="2" Margin="5">A text box</TextBox>
</Grid>
<Label Grid.Row="1">Some text in between the two grids...</Label>
<Grid
    Grid.Row="2"
    Margin="3"
    Background="LightYellow"
    ShowGridLines="True">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" SharedSizeGroup="TextLabel" />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>
    <Label Margin="5">Short</Label>
    <TextBox Grid.Column="1" Margin="5">A text box</TextBox>
</Grid>
</Grid>
```
</details>


>布局舍入
>避免元素边框发生锯齿现象
```xml
UseLayoutRounding="True"
```
## <span style="color:#0366d6;">UniformGrid</span>
>不需要预先定义行和列，只需要简单的通过Rows和Columns来确定行数和列数
```xml
<UniformGrid Rows="2" Columns="2">
      <Button>Top Left</Button>
      <Button>Top Right</Button>
      <Button>Bottom Left</Button>
      <Button>Bottom Right</Button>
</UniformGrid>
```
## <span style="color:#0366d6;">Canvas</span>
>可以使用数值进行绝对定位（单位设备无关单位），可以使用Canvas.Left,Canvas.Bottom,Canvas.Right,Canvas.Top设置数值来定义位置

```xml
 <Canvas>
      <Button Canvas.Left="10" Canvas.Top="10">(10,10)</Button>
      <Button Canvas.Left="120" Canvas.Top="30">(120,30)</Button>
      <Button Canvas.Left="60" Canvas.Top="80" Width="50" Height="50">(60,80)</Button>
      <Button Canvas.Left="70" Canvas.Top="120" Width="100" Height="50">(70,120)</Button>
 </Canvas>
```
## <span style="color:#0366d6;">InkCanvas</span>
>InkCanvas和Canvas类似，可以使用Canvas.Left,Canvas.Bottom,Canvas.Right,Canvas.Top设置数值来定义位置
但是InkCanvas不是派生自Canvas，甚至Panel，直接派生自FrameworkElement,主要用来接收手写设备还有鼠标的绘制
None = 0,Ink = 1,GestureOnly = 2,InkAndGesture = 3,Select = 4,EraseByPoint = 5,EraseByStroke = 6
```xml
<Grid>
      <Grid.RowDefinitions>
        <RowDefinition Height="Auto"></RowDefinition>
        <RowDefinition></RowDefinition>
      </Grid.RowDefinitions>
      <StackPanel Margin="5" Orientation="Horizontal">
        <TextBlock Margin="5">EditingMode: </TextBlock>
        <ComboBox Name="lstEditingMode"  VerticalAlignment="Center">          
        </ComboBox>
      </StackPanel>      
      <InkCanvas Name="inkCanvas" Grid.Row="1" Background="LightYellow" EditingMode="{Binding ElementName=lstEditingMode,Path=SelectedItem}">
        <Button InkCanvas.Top="10" InkCanvas.Left="10">Hello</Button>       
      </InkCanvas>
</Grid>
```