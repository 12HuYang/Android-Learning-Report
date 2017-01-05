# Android 判断一个 View 是否可见 getLocalVisibleRect(rect) 与 getGlobalVisibleRect(rect)

[TOC]

## 这两个方法的区别
- **View.getGlobalVisibleRect(rect); //以屏幕 左上角 为参考系的**
- **View.getLocalVisibleRect(rect);  //以目标 View 左上角 为参考系**

鉴于这一点的区别,View.getLocalVisibleRect(rect) 的 `rect.left `恒等于 0 .

## 判断是否可见

```
boolean localVisibleRect = target.getLocalVisibleRect(rect);
```

进入方法的源码可以看到:

```
    public final boolean getLocalVisibleRect(Rect r) {
        final Point offset = mAttachInfo != null ? mAttachInfo.mPoint : new Point();
        if (getGlobalVisibleRect(r, offset)) {
            r.offset(-offset.x, -offset.y); // make r local
            return true;
        }
        return false;
    }
```

**1. true : View 全部或者部分 可见**
**2. false : View 全部不可见**

在返回 true (即 View 全部或者部分 可见)时,`r.offset(-offset.x, -offset.y); // make r local`对 `rect` 的四个坐标进行了偏移.

如果是竖直的 Scrollview 里面的一个 View 在向上滑动的过程中,状态由 `全部可见 --> 部分可见 --> 全部不可见` ,其`Rect.top`的变化是:
注: 这里获取 View 的高度是 height 像素.

1. **全部可见 :** Rect.top 恒为 0;
2. **部分可见 :** Rect.top 的值 由 0 --> height
3. **全部不可见: ** 在全部不可见的瞬间,Rect.top 的值由 height 突变为 - height,其后随着滑动的距离越远,负值越大,建议自己打`log`查看下,这里没有截取 log,是因为 log 太多了.

> **总结: ** 当 Rect.top 的值不为 0 时,View 要么部分可见,要么完全不可见.那么当我们需要 View 在有一点点不可见时就返回 **false** 可以这样处理:

```
//当 View 有一点点不可见时立即返回false!
public static boolean isVisibleLocal(View target){
        Rect rect =new Rect();
        target.getLocalVisibleRect(rect);
        return rect.top==0;
    }
```

> 可以使用的场景还是很多的,比如类似淘宝滑动可以悬停在顶部的 View,当View 滑到有一点不可见时就需要把 外层的 悬浮View 显示出来,当滑动的 View 完全可见时才把 悬浮的View 隐藏掉.

## 最后

根据 返回的 Rect 我们其实还可以做更多的事,比如 计算正在 View 显示部分的面积等,只是目前不知道 求出面积 ,然后呢,干嘛呢....mdzz...=.=
