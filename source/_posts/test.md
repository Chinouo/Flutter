---
title: Widget详解
date: 2021-03-21
---
本文是个人对Flutter进行尝试后，想对框架进行更深一步的探究而写，纯新手，如有错误欢迎指正。

# Widget
第一次接触Flutter的Demo就看到各种Widget，学习中也掌握了各种Widget的使用方法，但是构建Widget的时候出了很多没见过的类型，下面就对这些内容进行解读。

 `Widget继承于DiagnosticableTree，是个抽象类。`(源码100行左右)

`Describes the configuration for an [Element].`

这个就是Widget存在的目的。
Widget是永远不变的，但是他可以inflated into(不知道怎么翻译合适)element，让其管理渲染树。

两个Widget的key和runtimeType如果==的话，那么新的Widget会在element那边取代掉旧的Widget。二者有一个不同的话，那么会造一个新的element。


这边列出几个重要的成员。
```dart
final Key

Element createElement();

static bool canUpdate(Widget oldWidget, Widget newWidget) {
    return oldWidget.runtimeType == newWidget.runtimeType
        && oldWidget.key == newWidget.key;
  }
```

## StatelessWidget








