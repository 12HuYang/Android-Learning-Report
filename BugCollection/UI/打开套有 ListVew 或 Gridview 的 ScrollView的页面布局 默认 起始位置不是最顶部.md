## 打开套有 ListVew 或 Gridview 的 ScrollView的页面布局 默认 起始位置不是最顶部

###方法一(推荐):
把套在里面的Gridview 或者 ListVew 不让获取焦点即可。

gridview.setFocusable(false);
listview.setFocusable(false);

**注意：在xml布局里面设置android：focusable=“false”不生效**

###方法二:
设置 myScrollView.smoothScrollTo(0,0);