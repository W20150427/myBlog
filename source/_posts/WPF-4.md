---
title: WPF 布局 应用篇
date: 2020-07-25
tags: [WPF]
categories: WPF
---
<!-- more -->
# <span style="color:#0366d6;">博客园首页布局</span>
>整个页面布局是一个五行三列的Grid,右边导航面板，可以用StackPanel,中间列表页是一个StackPanel
右边内容，放一个StatckPanel

![效果图](https://pic1.zhimg.com/v2-da2655ce59b0ac73a180240a3795c5f8_r.jpg)
<details>
<summary>点开查看</summary>
```xml
<Grid Margin="20">
<Grid.RowDefinitions>
<RowDefinition Height="30" />
<RowDefinition Height="80" />
<RowDefinition Height="40" />
<RowDefinition Height="*" />
<RowDefinition Height="170" />
</Grid.RowDefinitions>
<Grid.ColumnDefinitions>
<ColumnDefinition Width="160" />
<ColumnDefinition Width="*" />
<ColumnDefinition Width="300" />
</Grid.ColumnDefinitions>
<StackPanel
Grid.Row="0"
Grid.Column="0"
Orientation="Horizontal">
<Label HorizontalAlignment="Left" Content="代码改变世界" />
</StackPanel>
<StackPanel
Grid.Row="0"
Grid.Column="2"
HorizontalAlignment="Right"
Orientation="Horizontal">
<Label
  Grid.Row="0"
  HorizontalAlignment="Right"
  Content="登录" />
<Label
  Grid.Row="0"
  HorizontalAlignment="Right"
  Content="注册" />
</StackPanel>
<StackPanel
Grid.Row="1"
Grid.Column="0"
HorizontalAlignment="Left"
Orientation="Horizontal">
<Image Source="logo_small.gif" Stretch="None" />
</StackPanel>
<DockPanel
Grid.Row="2"
Grid.ColumnSpan="3"
Background="#2b6695">
<StackPanel HorizontalAlignment="Left" Orientation="Horizontal">
  <Label
      Margin="5,0,0,0"
      VerticalAlignment="Center"
      Background="#2b6695">
      园子
  </Label>
  <Label
      Margin="5,0,0,0"
      VerticalAlignment="Center"
      Background="#2b6695">
      新闻
  </Label>
  <Label
      Margin="5,0,0,0"
      VerticalAlignment="Center"
      Background="#2b6695">
      博问
  </Label>
  <Label
      Margin="5,0,0,0"
      VerticalAlignment="Center"
      Background="#2b6695">
      闪存
  </Label>
  <Label
      Margin="5,0,0,0"
      VerticalAlignment="Center"
      Background="#2b6695">
      网摘
  </Label>
  <Label
      Margin="5,0,0,0"
      VerticalAlignment="Center"
      Background="#2b6695">
      招聘
  </Label>
  <Label
      Margin="5,0,0,0"
      VerticalAlignment="Center"
      Background="#2b6695">
      专题
  </Label>
  <Label
      Margin="5,0,0,0"
      VerticalAlignment="Center"
      Background="#2b6695">
      知识
  </Label>
</StackPanel>
</DockPanel>

<StackPanel
Grid.Row="3"
Grid.Column="0"
Orientation="Vertical">
<StackPanel Orientation="Vertical">
  <Border
      BorderBrush="#2b6695"
      BorderThickness="1"
      CornerRadius="3"
      UseLayoutRounding="True">
      <StackPanel Orientation="Vertical">
          <Label Content=".Net技术" />
          <Label Content="编程语言" />
          <Label Content="软件设计" />
          <Label Content="Web前端" />
          <Label Content="手机开发" />
          <Label Content="软件工程" />
          <Label Content="数据库技术" />
          <Label Content="操作系统" />
          <Label Content="其他分类" />
          <Label Content="所有随笔" />
          <Label Content="所有评论" />
      </StackPanel>
  </Border>
  <Border
      Margin="0,20,0,0"
      BorderBrush="#2b6695"
      BorderThickness="1"
      CornerRadius="3"
      UseLayoutRounding="True">
      <StackPanel Orientation="Vertical">
          <Label Content="反馈或建议" />
          <Label Content="官方博客" />
          <Label Content="博客模板" />
          <Label Content="Java博客" />
          <Label Content="C++博客" />
          <Label Content="手机版" />
      </StackPanel>
  </Border>
</StackPanel>
</StackPanel>
<StackPanel
Grid.Row="3"
Grid.Column="1"
Margin="20"
Orientation="Vertical">
<Border
  Margin="0,10,0,0"
  BorderBrush="DarkGray"
  BorderThickness="0,0,0,1">
  <StackPanel Orientation="Vertical">
      <StackPanel Orientation="Horizontal">
          <StackPanel>
              <Image Source="20200725165013.png" />
          </StackPanel>
          <StackPanel Orientation="Vertical">
              <Label Content="CPU瞒着内存竟干出这种事" />
              <StackPanel Orientation="Horizontal">
                  <Image Source="20200512215055.png" />
                  <Label Content="简介..." />
              </StackPanel>
              <Label Content="CPU轩辕之风 发布于 2020-05-15 14:22" />
          </StackPanel>
      </StackPanel>
  </StackPanel>
</Border>
<Border
  Margin="0,10,0,0"
  BorderBrush="DarkGray"
  BorderThickness="0,0,0,1">
  <StackPanel Orientation="Vertical">
      <StackPanel Orientation="Horizontal">
          <StackPanel>
              <Image Source="20200725165013.png" />
          </StackPanel>
          <StackPanel Orientation="Vertical">
              <Label Content="CPU瞒着内存竟干出这种事" />
              <StackPanel Orientation="Horizontal">
                  <Image Source="20200512215055.png" />
                  <Label Content="简介..." />
              </StackPanel>
              <Label Content="CPU轩辕之风 发布于 2020-05-15 14:22" />
          </StackPanel>
      </StackPanel>
  </StackPanel>
</Border>
<Border
  Margin="0,10,0,0"
  BorderBrush="DarkGray"
  BorderThickness="0,0,0,1">
  <StackPanel Orientation="Vertical">
      <StackPanel Orientation="Horizontal">
          <StackPanel>
              <Image Source="20200725165013.png" />
          </StackPanel>
          <StackPanel Orientation="Vertical">
              <Label Content="CPU瞒着内存竟干出这种事" />
              <StackPanel Orientation="Horizontal">
                  <Image Source="20200512215055.png" />
                  <Label Content="简介..." />
              </StackPanel>
              <Label Content="CPU轩辕之风 发布于 2020-05-15 14:22" />
          </StackPanel>
      </StackPanel>
  </StackPanel>
</Border>
<Border
  Margin="0,10,0,0"
  BorderBrush="DarkGray"
  BorderThickness="0,0,0,1">
  <StackPanel Orientation="Vertical">
      <StackPanel Orientation="Horizontal">
          <StackPanel>
              <Image Source="20200725165013.png" />
          </StackPanel>
          <StackPanel Orientation="Vertical">
              <Label Content="CPU瞒着内存竟干出这种事" />
              <StackPanel Orientation="Horizontal">
                  <Image Source="20200512215055.png" />
                  <Label Content="简介..." />
              </StackPanel>
              <Label Content="CPU轩辕之风 发布于 2020-05-15 14:22" />
          </StackPanel>
      </StackPanel>
  </StackPanel>
</Border>
<Border
  Margin="0,10,0,0"
  BorderBrush="DarkGray"
  BorderThickness="0,0,0,1">
  <StackPanel Orientation="Vertical">
      <StackPanel Orientation="Horizontal">
          <StackPanel>
              <Image Source="20200725165013.png" />
          </StackPanel>
          <StackPanel Orientation="Vertical">
              <Label Content="CPU瞒着内存竟干出这种事" />
              <StackPanel Orientation="Horizontal">
                  <Image Source="20200512215055.png" />
                  <Label Content="简介..." />
              </StackPanel>
              <Label Content="CPU轩辕之风 发布于 2020-05-15 14:22" />
          </StackPanel>
      </StackPanel>
  </StackPanel>
</Border>
<Border
  Margin="0,10,0,0"
  BorderBrush="DarkGray"
  BorderThickness="0,0,0,1">
  <StackPanel Orientation="Vertical">
      <StackPanel Orientation="Horizontal">
          <StackPanel>
              <Image Source="20200725165013.png" />
          </StackPanel>
          <StackPanel Orientation="Vertical">
              <Label Content="CPU瞒着内存竟干出这种事" />
              <StackPanel Orientation="Horizontal">
                  <Image Source="20200512215055.png" />
                  <Label Content="简介..." />
              </StackPanel>
              <Label Content="CPU轩辕之风 发布于 2020-05-15 14:22" />
          </StackPanel>
      </StackPanel>
  </StackPanel>
</Border>
<Border
  Margin="0,10,0,0"
  BorderBrush="DarkGray"
  BorderThickness="0,0,0,1">
  <StackPanel Orientation="Vertical">
      <StackPanel Orientation="Horizontal">
          <StackPanel>
              <Image Source="20200725165013.png" />
          </StackPanel>
          <StackPanel Orientation="Vertical">
              <Label Content="CPU瞒着内存竟干出这种事" />
              <StackPanel Orientation="Horizontal">
                  <Image Source="20200512215055.png" />
                  <Label Content="简介..." />
              </StackPanel>
              <Label Content="CPU轩辕之风 发布于 2020-05-15 14:22" />
          </StackPanel>
      </StackPanel>
  </StackPanel>
</Border>
<Border
  Margin="0,10,0,0"
  BorderBrush="DarkGray"
  BorderThickness="0,0,0,1">
  <StackPanel Orientation="Vertical">
      <StackPanel Orientation="Horizontal">
          <StackPanel>
              <Image Source="20200725165013.png" />
          </StackPanel>
          <StackPanel Orientation="Vertical">
              <Label Content="CPU瞒着内存竟干出这种事" />
              <StackPanel Orientation="Horizontal">
                  <Image Source="20200512215055.png" />
                  <Label Content="简介..." />
              </StackPanel>
              <Label Content="CPU轩辕之风 发布于 2020-05-15 14:22" />
          </StackPanel>
      </StackPanel>
  </StackPanel>
</Border>
</StackPanel>
<StackPanel
Grid.Row="3"
Grid.Column="2"
Orientation="Vertical">
<StackPanel Orientation="Vertical">
  <Border
      BorderBrush="#404040"
      BorderThickness="1"
      UseLayoutRounding="True">
      <StackPanel Orientation="Vertical">
          <Label Content="新闻1" />
          <Label Content="新闻2" />
          <Label Content="新闻3" />
          <Label Content="新闻4" />
          <Label Content="新闻5" />
          <Label Content="新闻6" />
          <Label Content="新闻7" />
          <Label Content="新闻8" />
          <Label Content="新闻9" />
          <Label Content="新闻10" />
          <Label Content="新闻11" />
      </StackPanel>
  </Border>
  <Border
      Margin="0,20,0,0"
      BorderBrush="#404040"
      BorderThickness="1"
      UseLayoutRounding="True">
      <StackPanel Orientation="Vertical">
          <Label Content="新闻12" />
          <Label Content="新闻13" />
          <Label Content="新闻14" />
          <Label Content="新闻15" />
      </StackPanel>
  </Border>
</StackPanel>
</StackPanel>
</Grid>
```
</details>