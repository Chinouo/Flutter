---
title: myfirstRender
date: 2021-05-12 21:59:32
tags:
---

# 手写瀑布流
### 第一次手写瀑布流失败了，倒在了GC和性能上面。但是总结了很多关于Render和Layout的经验。
[1]RenderSliverMultiBoxAdaptor这个类是手写瀑布流的关键。

[2]SliverGeometry输出必要信息给给ViewPort，ViewPort会传入constraints给RenderSliver。

[3]perforLayout管理元素的布局信息，每次更新都要对RenderOBJ重新layout。

[4]paint方法是画在屏幕上的关键。

[5]对SliverList的布局过程有了深刻的了解。诸如insertAndLayout,collectGarbage等等。

## 以下是成功实现上滑GC和下滑布局的代码。
```dart
    //在生成新的并进行layout
    RenderBox min = getTrailingChild()!;
    while (true) {
      if (childScrollOffset(min)! > targetEndScrollOffset) {
        break;
      }
      RenderBox? child = insertAndLayoutChild(childConstraints,
          after: lastChild, parentUsesSize: true);

      if (child == null) break;

      final data = min.parentData as FlowSliverParentData;

      final childData = child.parentData as FlowSliverParentData;
     
      childData.crossAxisPosition = data.crossAxisPosition;
      childData.layoutOffset = paintExtentOf(min) + data.layoutOffset!;

      if (childData.crossAxisPosition == 0) {
        _noteBookofIndex.left.add(childData.index);
      } else {
        _noteBookofIndex.right.add(childData.index);
      }

      min = getTrailingChild()!;
    }
```
```dart
   //视窗上滑回收下方元素
    RenderBox? child = firstChild;
    int trailingGarbage = 0;
    while (child != null) {
      if (targetEndScrollOffset < childScrollOffset(child)!) {
        trailingGarbage += 1;
      }
      child = childAfter(child);
    }
```
