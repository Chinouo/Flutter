---
title: state
date: 2021-03-22 22:49:11
---
# State类(源码400行)
`使用Diagnosticable接口`
其属性在使用initState前被框架调用。

State和BuildContext有着永久的联系，State不会改变BuildContext，但是BuildContext可以在树中任意移动。(?)
--
```dart
  BuildContext get context {
   ......
       return _element!;
  }
```
该方法返回一个element。

只要创建了State，在调用initState之前，框架会通过BuildContext `mounts`(不知道怎么翻译)State对象，调用了dispose则框架就不会对State进行再一次build了。

---

```dart
  @protected
  @mustCallSuper
  void initState() {
    assert(_debugLifecycleState == _StateLifecycle.created);
  }
```
熟悉的initState，框架在其插入到树中时调用，且只调用一次。

当State的build方法中有依赖于其他带State的Widget时，确保在`initState` (订阅)， `didUpdateWidget`(取关旧的obj，订阅新的obj),  `dispose`(取关) 中对 notifications 进行正确的订阅或者取关(unsubscribe)。

文档中有关于`didChangeDependencies`和`BuildContext.dependOnInheritedWidgetOfExactType`的联系。(不懂，暂时放着)

---

```dart
  @mustCallSuper
  @protected
  void didUpdateWidget(covariant T oldWidget) {}
  //covariant关键字
```
只要配置信息改变，就会调用这个方法。
这个方法也会触发build的调用。重写定义Widget更改时的行为，比如动画什么的。重写时注意调用`super.didUpdateWidget(oldWidget)`。

---
```dart
  @protected
  @mustCallSuper
  void reassemble() {}
```
关于widgets重组的部分(暂时不懂)。

---
# `setState`(重要内容)
```dart
 void setState(VoidCallback fn) {
     ...//assert关于生命周期的部分
    final dynamic result = fn() as dynamic;

    ...//assert部分关于result是不是Future部分

    //更新部分
    _element!.markNeedsBuild();
 }
```
不建议在setState里包含太多运算过程。可通过State是否为mounted来进行合法使用该方法。

---

```dart
  @protected
  @mustCallSuper
  void deactivate() {}
```
该方法在对象从树中移除时调用。
文档中有框架对树嫁接时对该方法的处理过程。(看不懂，暂时打住)

```dart
  @protected
  @mustCallSuper
  void dispose() {
    assert(_debugLifecycleState == _StateLifecycle.ready);
    assert(() {
      _debugLifecycleState = _StateLifecycle.defunct;
      return true;
    }());
  }
```
当State确定永远不会build时，调用该方法永久销毁。

# `build`方法
描述通过Widget配置的用户界面。

在以下多种情况被调用。

### (1)`initState`被调用后

### (2)`didUpdateWidget`被调用后

### (3)`setState`被调用后

### (4)`State`的依赖(`dependency`)更改时

### (5)`deactivate`调用后又重新插入树中

框架调用这个的返回值来更新子树，是否更新某个根节点取决于是否调用Widget.canUpdate的方法。
`BuildContext`包含树的位置，并且总是和State的context属性相同。

# build的设计思路
## `为什么把build设计在State内而不是StatefulWidget？`
官方解释： gives developers more flexibility when subclassing
[StatefulWidget].

问题1：如果带State参数的build在StatefulWidget，如果子类不需要State，但是重载的缘故你又不得不写个State给子类build。(有点晦涩，先留个印象吧)
问题2：父类重构时，无法使用更新后的状态。(没搞懂)

---
```dart
  @protected
  @mustCallSuper
  void didChangeDependencies() {}

```
这个方法在dependency更改时调用，也会在initState调用后执行。一般在更换依赖或者build开销大的时候才重写这个方法。
