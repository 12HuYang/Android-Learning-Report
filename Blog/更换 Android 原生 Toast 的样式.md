# 更换 Android 原生 Toast 的样式

> Toast 每个用Android 手机的用户再熟悉不过,而且各种自定义样式的 文章也层出不穷
> 本篇文章并不是要走他们的老路,本文只针对 Toast 的UI样式,剖析完后再来定制.
> 本文源码:**[https://github.com/didikee/CommonDependence](https://github.com/didikee/CommonDependence)**
> 觉得不错可以先关注下,后面会更新更多的组件 ๑乛◡乛๑

## Toast 使用的 Layout
在源码中写到:
```
View v = inflate.inflate(com.android.internal.R.layout.transient_notification, null);
```
也就是:
**`com.android.internal.R.layout.transient_notification`**

其布局文件如下:
```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="?android:attr/toastFrameBackground">

    <TextView
        android:id="@android:id/message"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:layout_gravity="center_horizontal"
        android:textAppearance="@style/TextAppearance.Toast"
        android:textColor="@color/bright_foreground_dark"
        android:shadowColor="#BB000000"
        android:shadowRadius="2.75"
        />

</LinearLayout>
```
文件路径在:**`...\SDK\platforms\android-23\data\res\layout\transient_notification.xml`**

**需要一个个解读的如下: **
```
1. Toast 的背景 : android:background="?android:attr/toastFrameBackground"
2. Toast 文字样式 : android:textAppearance="@style/TextAppearance.Toast"
3. Toast 文字颜色 : android:textColor="@color/bright_foreground_dark"
```

## Toast 的背景 Background
`android:background="?android:attr/toastFrameBackground`

在**`...\SDK\platforms\android-23\data\res\values\themes.xml`**中,可以看到,背景其实是一个 `Drawable`资源
```
        <!-- Toast attributes -->
        <item name="toastFrameBackground">@drawable/toast_frame</item>
```
最终在:**`...\SDK\platforms\android-23\data\res\drawable-xxhdpi\toast_frame.9.png`**找到了

![Toast 背景实际是点9图](pic/toast_frame.9.png)

本来是想改变`.9图`的颜色的,但是Google了很久都没有解决方案,可能 `SVG矢量图`是为了弥补这一缺陷而运用在Android上的.

如果谁有解决方案,麻烦@下我,我也想知道 ๑乛◡乛๑
好了,换一个思路.

经过 [马克曼](http://www.getmarkman.com/)的测量,再配合 `Material Design`的设计规范,得到了最终满意的尺寸.

MD 习惯的尺寸为 1,2,4,8,16,24,36,48,56......绝大数情况是偶数[单位都是dp]

最终布局如下:
```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:background="@drawable/shape_toast"
              android:orientation="vertical">
    <TextView
        android:id="@android:id/message"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingBottom="12dp"
        android:paddingTop="12dp"
        android:paddingLeft="24dp"
        android:paddingRight="24dp"
        android:textSize="16sp"
        android:layout_gravity="center_horizontal"
        android:layout_weight="1"
        android:shadowColor="#BB000000"
        android:shadowRadius="2.75"
        android:textColor="@color/WHITE"
        />

</LinearLayout>
```

## Toast 文字样式

`android:textAppearance="@style/TextAppearance.Toast`
文件路径: **`...\SDK\platforms\android-23\data\res\values\style.xml`**
```
    <style name="TextAppearance.Toast">
        <item name="fontFamily">sans-serif-condensed</item>
    </style>
```
设置 `` 有两种方式:
```
1. textView.setTextAppearance(R.style.AndroidToast);//api16
2. Typeface typeface = Typeface.create("sans-serif-condensed", Typeface.NORMAL);
            textView.setTypeface(typeface);//api1
```
第一种看起来方便简洁,但是要求 api>=16,而 4.0 对应的是 api15,所以还是选择第二种吧


## Toast 文字颜色
```
android:textColor="@color/bright_foreground_dark"
<color name="bright_foreground_dark">@android:color/background_light</color>
<color name="background_light">#ffffffff</color>
```

所以默认的颜色是纯白的 `#FFFFFF`

## 最后贴下 java 代码
```
    /**
     * {@link layout/transient_notification.xml}
     * @param content content to show
     * @param longTime short or long
     * @param context context
     * @param textColor toast text color
     * @param toastBackgroundColor toast background color
     */
    public static void showToast(@NonNull Context context, String content, boolean longTime, @ColorInt
            int textColor,@ColorInt int toastBackgroundColor) {
        int type = longTime ? Toast.LENGTH_LONG : Toast.LENGTH_SHORT;
        Toast toast = Toast.makeText(context, content, type);
        ViewGroup toastView = (ViewGroup) LayoutInflater.from(context).inflate(R.layout
                .layout_toast, null, false);
        if (toastBackgroundColor != 0) {
            toastView.setBackgroundDrawable(getToastBackground(context, toastBackgroundColor));
        }
        TextView textView = (TextView) toastView.findViewById(android.R.id.message);
        // 内部已经作非空判断了
        if (textColor!=0){
            textView.setTextColor(textColor);
        }
        Typeface typeface = Typeface.create("sans-serif-condensed", Typeface.NORMAL);
        textView.setTypeface(typeface);
        toast.setView(toastView);
        toast.setText(content);
        toast.show();
    }

    private static Drawable getToastBackground(@NonNull Context context, @ColorInt int color) {
        GradientDrawable gradientDrawable = new GradientDrawable();
        gradientDrawable.setShape(GradientDrawable.RECTANGLE);
        gradientDrawable.setCornerRadius(DisplayUtil.dp2px(context, 24));
        gradientDrawable.setColor(color);
        return gradientDrawable;
    }
```

## 结束
> 本文源码:**[https://github.com/didikee/CommonDependence](https://github.com/didikee/CommonDependence)**
> 觉得不错可以先关注下,后面会更新更多的组件 ๑乛◡乛๑
















