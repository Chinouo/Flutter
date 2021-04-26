---
title: future
date: 2021-04-26 14:45:38
tags:
---

# Future
这部分涉及到Dart的异步调用，Future类似于JS的Promise，一般在网络请求中需要使用异步。
## 事件循环概念
Dart是单线程的，main方法就是一个Isolate。Isolate这里不做展开。

Dart线程中有一个消息循环机制，如下图。
![avatar](\img\Task&Event.png)
可以这样理解，有Future的事件会被放到Event队列，没有就放到MicroTask，比如你的各种+-=操作。

## 延迟任务
```dart
    Future.delayed(Duration(seconds:1),(){
        print("延迟1S");
    });
        Future.delayed((){
        sleep(Duration(seconds:5));
        print("延迟5S");
    });
```
此时是5S的先输出,1S的再输出。因为1s的任务再1s才会放到Event去，而Sleep则是直接推到Event。

## 创建Future
```dart
    Future();
    Future.microtask();
    Future.sync(); //同步方法，立即执行。
    Future.value();
    Future.delayed();
    Future.error();
```

## Future的回调注册
学过Promise的同学知道resolve(),reject(),then()等用法，这里也是类似的。
```dart
    Future f = Future.value("100");
    f.then( (value){
        print(value); // 输出100
    }).then((){
        print("链式调用!和JS一样");
    }).catchError((e){
        print(e);   //一般在then后捕获异常，JS有个说法是关于Promise的异常捕获和常规的有点不一样。
    });

    Future f2 = Future.wait( [f] );  //等待多个Future完成后再执行任务 这边f的值一般是注册回调中return的值，也就是value
    f2.then( (){
        // do something
    });



    print("hello");
    //最终输出顺序hello 100 链式...
```
## async await修饰符
和JS一样。异步函数,dart中把async修饰的方法包装成Future,同时没有async修饰await无法使用。
```dart
    func()async{
        var x = await "1";
        print(x);               //等await后的语句执行结束，才执行下一句。
    }
```

### 泛型Future
```dart
    Future<String> f;
    //泛型是你期望的返回值。
```


