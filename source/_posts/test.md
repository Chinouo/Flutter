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

createState在StatefulWidget在树中移除，之后再次插入时调用，创建一个新的State。

使用GlobalKey可以在移动Widget树时保持State对象，文档中说这样做不会重新在子树构建element，而是直接复用该element嫁接上去。

## 性能问题
### 两种基础类型的StatefulWidget
`第一种：在initState中分配资源，在dispose销毁资源，但不使用InheritedWidget，setState`

性能开销小，构建一次但不会再更新内容。

`第二种：使用InheritedWidget，setState`

经常在rebuild。

### 解决方案
(1)把State送给叶子节点让他更新，防止整个Widget重构。

(2)简化树的深度广度，一个StatefulWidget尽量不要build太多其他Widget。

(3)如果子树不变，缓存起来重用。一般把其封装到一个child参数里面。(这部分文档有点难懂)

(4)使用const

(5)避免更改已经创建好的子树深度或者其里面任何一个Widget类型。

(6)非得改变树的深度，那么可以用GlobalKey，把共有子树部分用Widget包装。

**道理很简单，能复用就复用，能不变就不变。**

`_StateLifecycle枚举类型有  created, initialized,  ready,  defunct四种，分别对应State的创建，init，已经build，已经dispose，供debug使用。`

# ProxyWidget
## ParentDataWidget
## InheritedWidget

# RenderObjectWidget
## LeafRenderObjectWidget
## SingleChildRenderObjectWidget
## MultiChildRenderObjectWidget

