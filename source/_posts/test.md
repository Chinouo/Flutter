---
title: Flutter Widget详解
date: 2021-03-21
---
本文是个人对Flutter进行尝试后，想对框架进行更深一步的探究而写，纯新手，如有错误欢迎指正。

# Widget
第一次接触Flutter的Demo就看到各种Widget，学习中也掌握了各种Widget的使用方法，但是构建Widget的时候出了很多没见过的类型，下面就对这些内容进行解读。


## StatelessWidget
```dart
class name extends StatelessWidget {
    const name({Key key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(     
    );
  }
}
```
这是Awesome Flutter Snippets插件自动生成的内容，出现了Key形参,build方法和BuildContext类型，稍后会做详解，这里我们对StatelessWidget源码进行解析(6K行，大部分都是注释)。(全部都在framework.dart,key在key.dart)




# framework.dart
## Key的种类(这两个是继承LocalKey)

## LocalKey


UniqueKey
ObjectKey





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

