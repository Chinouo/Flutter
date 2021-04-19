---
title: intl
date: 2021-04-19 22:40:51
tags:
---
# INTL

## 安装教程
1. pub获得INTL插件
2. dependencies中配置sdk
3. flutter中添加generate:true
配置成功后会在.dart_tool下生成flutter_gen文件
---


## 配置过程
1. 创建对应arb文件
2. 创建L10n.dart文件用于保存支持的语言
3. 类内import gen_l10n下的dart文件，以及L10n.dart文件
4. 在MaterialApp中设置
```dart
//上面是用Provider实现的数据管理
MaterialApp(
                  locale: localeprovider.locale,
                  supportedLocales: L10n.all,
                  localizationsDelegates: [
                    AppLocalizations.delegate,
                    GlobalCupertinoLocalizations.delegate,
                    GlobalMaterialLocalizations.delegate
                  ])//设置对应组件的多语言渲染
```
5. 在Widget中使用下面方法构建
```dart
    Text(AppLocalizations.of(context).output),
```