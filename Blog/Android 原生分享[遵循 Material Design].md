# Android 原生分享[遵循 Material Design]

> 原生分享一般用于个人,或者一些偏向极客一点的公司,一般的电商等主要是使用 umeng等提供的第三方分享服务,优缺点不说了.下面开始分享使用 `BottomSheetsDialog` 实现的遵循 MD 的分享样式.
>
>你可以在这里获取源码: **[https://github.com/didikee/CommonDependence](https://github.com/didikee/CommonDependence)**
> 觉得不错可以先关注下,后面会更新更多的组件 ๑乛◡乛๑

## 默认的分享

使用默认的分享也很简单,比如分享一段文本信息:

```
    public void shareDefaultText(@NonNull Activity activity, String dialogTitle, String
            shareTitle, String shareContent) {
        Intent share_intent = new Intent();
        share_intent.setAction(Intent.ACTION_SEND);//设置分享行为
        share_intent.setType("text/plain");//设置分享内容的类型 text/plain
        share_intent.putExtra(Intent.EXTRA_SUBJECT, shareTitle);//添加分享内容标题
        share_intent.putExtra(Intent.EXTRA_TEXT, shareContent);//添加分享内容
        shareDefault(activity, share_intent, dialogTitle);
    }
    
        public void shareDefault(@NonNull Activity activity, Intent shareIntent, String dialogTitle) {
        //创建分享的Dialog
        try {
            shareIntent = Intent.createChooser(shareIntent, dialogTitle);
            activity.startActivity(shareIntent);
        } catch (Exception e) {
            // error
            // sometime , there is no app to share
            Toast.makeText(activity, "分享失败", Toast.LENGTH_SHORT).show();
        }
    }
```

## 使用 BottomSheetsDialog分享

这里给一张对比图:(左边是 BottomSheetsDialog实现,右边是默认)
![对比图](pic/device-原生分享ui-1.png)

代码在:**[源代码点击这里](https://github.com/didikee/CommonDependence/tree/master/dependence)**

对于 `BottomSheetsDialog`不熟悉的可以看这篇文章 [BottomSheetsDialog详解](http://www.jianshu.com/p/0a7383e0ad0f#)

**贴在这里感觉比较乱,所以就不贴,实现很简单:**
```
1. BottomSheetsDialog
2. NestedScrollView
3. Recyclerview 
4. RecyclerView.ItemDecoration
```
## 注意点说一下:
```
1. Scrollview 嵌套 RecyclerView 会不能 fling 快速滑动,使用 NestedScrollView 代替 Scrollview 即可.
2. RecyclerView.ItemDecoration 中获取 item的个数,应该用:
int childCount = parent.getLayoutManager().getItemCount();
而不是:
int childCount = parent.getChildCount();/*这个得到的是1,2,3,4....的变值*/
```



