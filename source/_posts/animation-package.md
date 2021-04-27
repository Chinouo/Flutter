---
title: animation_package
date: 2021-04-27 17:05:40
tags:
---
# 官方Anmation包

## OpenContainer实现过渡动画
```dart
//把要实现跳转的组件放在closeBuilder
//通过openContainear调用openBuilder方法实现路由跳转，这比Hero动画燃多了。
OpenContainer(
      transitionDuration: Duration(seconds: 1),
      openBuilder: (context, action) => LangPickPage(),
      closedBuilder: (context, VoidCallback openContainer) => GestureDetector(
        onTapUp: (details) => openContainer() ,
        child: ...
      );
```