---
title: provider
date: 2021-04-19 22:25:09
tags:
---

# Provider
状态管理的一个插件，通过InheritWidget等方法实现状态更新。

## 实现过程
首先写一个Provider类继承于ChangeNotifier，通过方法内调用notifyListener()来通知更新。

## 最顶层
在需要数据的组件上层，Wrap一个ChangeNotifierProvider，
其中create形参传入数据Provider对象，builder形参构建所需组件。

## 数据更新
通过使用如下方法访问数据
```dart
      final localeprovider = Provider.of<LocaleProvider>(context, listen: false);
      localeprovider.setLocale(L10n.all[1]);
```
## 多个Provider情况
防止多层provider的嵌套对可读性造成的影响。
```dart
       Widget build(BuildContext context) {
    return MultiProvider(
      providers: [
        ChangeNotifierProvider(create: (_) => LocaleProvider()),
        ChangeNotifierProvider(create: (_) => ThemeProvider()),
      ],
      builder: (context, child) {
        final themeProvider = Provider.of<ThemeProvider>(context);
        final localeprovider = Provider.of<LocaleProvider>(context);
        return MaterialApp(...);
```
