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

