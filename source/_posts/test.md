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
```dart
StatelessElement createElement() => StatelessElement(this);

  @protected
  Widget build(BuildContext context);
```
### createElement方法
创造管理widget在树中位置的方法。



### build方法
这个很关键，框架通过调用这个方法把Widget插入到element树中。

在三种情况下会调用这个方法：
(文档也同时给吃了优化方案)

1：首次把Widget插入到element树中。

2：当Widget的父元素更换其配置信息

3：当一个InheritedWidget的依赖更改。
#### BuildContext参数
包含了关于组件应该在树中何处被创建，一个Widget可能是由多个不同的BuildContext组成，比如widget一次性在树的多个位置被插入或者移动。

# StatefulWidget
创建的是有一般是要写两个类，一个私有的管理State的类，一个用来调用creatState的类(有时候可以通过Stream，ChangeNotifier实现对State的订阅)。build过程是递归的。StatefulWidget是不变的，但State可变的。一个StatefulWidget可以关联多个State对象。

State的信息在Widget build的时候可以同步调用，在Widget的生命周期内可以更换，用State.setState来更新状态。

createState在StatefulWidget在树中移除，之后在插入时调用，创建一个新的State。






