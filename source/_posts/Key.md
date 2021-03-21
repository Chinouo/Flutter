---
title: Key
date: 2021-03-21 15:49:08
tags:
---


# key.dart (100行源码)
## 什么是Key?(官方解释)
[官方视频](https://www.youtube.com/watch?v=kn0EOS-ZiIc)

A [Key] is an identifier for [Widget]s, [Element]s and [SemanticsNode]s.
(这个就不做翻译了，懂的都懂)


每个Widget都有一个Key,

一个新的Widget有个Key,

旧的Widget也有Key

旧的Widget与某个element有关联,

当新旧Widget的Key相同时,那么新的Widget会被用来更新element。

key只等于自己，也就是一个key是独一无二的。
### Key是个抽象类 

```dart
@immutable
abstract class Key {

  const factory Key(String value) = ValueKey<String>;

  @protected
  const Key.empty();
}
```
有的同学就要问了，factory是什么玩意。
看过JS红宝书的工厂模式就知道，制造对象用的。

文档还提供了LocalKey以及其子类ValueKey<T>的实现(后者有类型合法校验，至于用法查看文档，这里只是留个印象)




# framework.dart
## 这个文档提供了

### UniqueKey
创建一个独一无二的Key,不能使用常量创建。
### ObjectKey
把对象当作自身值的Key
### GlobalKey
提供访问与element相关的元素，比如BuildContext，StatefulWidget的State。用该Key重塑Element树开销很大(文档这么写的)，两个Widget不能用同一个GlobalKey，GlobalKey不建议在build内重新创建，因为这样会丢弃掉原来的State，创建新的子树，也可能导致意外的组件行为。建议是用initState创建并长久保存。

源码中有注册和追踪Element变化的方法和变量，具体看源码。



### LabeledGlobalKey
提供调试用的GlobalKey
### GlobalObjectKey
绑定一个用来生成Widget的对象，用对象作为形参。文档建议用为私有，防止冲突。