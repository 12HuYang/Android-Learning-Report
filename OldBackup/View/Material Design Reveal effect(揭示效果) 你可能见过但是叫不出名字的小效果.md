# Material Design Reveal effect(揭示效果) 你可能见过但是叫不出名字的小效果

> 前言: 每次写之前都会来一段(废)话.{心塞...}
> Google Play首页两个tab背景用了这个效果,三星计算器用了这个效果,酷安也看见这个效果,但就是叫不出名字!!!抓狂啊!!!

> 没办法,由于这个效果类似 `涟漪效果`,所以我就用** Ripple **为关键字,找过**RippleDrawable** ,但是没发现...最后在Google的帮助下,我从一个陌生的网站看到了**`Reveal Effect`**中文翻译为:揭示效果(翻译不对你来咬我啊,哈哈哈2333)
> 好了,废话不多说了,先来看个效果吧.

![Reveal Effect.gif](http://oidz2h9ht.bkt.clouddn.com//blog/2016/12/18/Reveal%20effect.gif)
ps:图是studio录的mp4,然后转的,gif有点卡顿感,实际效果很顺滑

### 核心代码

```
ViewAnimationUtils.createCircularReveal(
View view,//目标view
int centerX,//开始动画的起点x坐标(相对于目标view而言)
int centerY,//开始动画的起点y坐标(相对于目标view而言)
float startRadius,//初始半径
float endRadius//结束半径
);
```

官方文档: [ViewAnimationUtils.createCircularReveal](https://developer.android.com/reference/android/view/ViewAnimationUtils.html#createCircularReveal)

### example展示

**1. activity layout 布局**

```
<LinearLayout
    android:id="@+id/activity_reveal_effect"
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.didikee.demos.ui.activity.RevealEffectActivity">

    <View
        android:id="@+id/view"
        android:layout_width="match_parent"
        android:layout_height="250dp"
        android:background="@color/colorAccent"
        />
    <Button
        android:id="@+id/bt_action"
        android:text="展开/收缩"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

</LinearLayout>
```

**2. java 代码 **

```
    private View view;
    private double radio;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_reveal_effect);
        view = findViewById(R.id.view);
        findViewById(R.id.bt_action).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                createAnimation(view).start();
            }
        });
    }

    @TargetApi(Build.VERSION_CODES.LOLLIPOP)
    public Animator createAnimation(View v) {

        Animator animator;
        if (radio == 0L) {
            radio = Math.sqrt(Math.pow(view.getWidth(), 2) + Math.pow(view.getHeight(), 2));
        }
        if (isOn) {
            animator = ViewAnimationUtils.createCircularReveal(
                    v,// 操作的视图
                    0,// 动画开始的中心点X
                    0,// 动画开始的中心点Y
                    (float) radio,// 动画开始半径
                    0);// 动画结束半径
        } else {
            animator = ViewAnimationUtils.createCircularReveal(
                    v,// 操作的视图
                    0,// 动画开始的中心点X
                    0,// 动画开始的中心点Y
                    0,// 动画开始半径
                    (float) radio);// 动画结束半径
        }
        animator.addListener(new Animator.AnimatorListener() {
            @Override
            public void onAnimationStart(Animator animation) {
                if (!isOn) {
                    view.setVisibility(View.VISIBLE);
                }
            }

            @Override
            public void onAnimationEnd(Animator animation) {
                if (isOn) {
                    view.setVisibility(View.INVISIBLE);
                }
                isOn = !isOn;
            }

            @Override
            public void onAnimationCancel(Animator animation) {

            }

            @Override
            public void onAnimationRepeat(Animator animation) {

            }
        });
        animator.setInterpolator(new DecelerateInterpolator());
        animator.setDuration(500);
        return animator;
    }
    private boolean isOn = true;//记录view的状态,第一次进去是可见的,记为true,不可见记为false
```

### 结束

**代码很简单,应该有和我一样,见过这个效果但是说不出名字,知道的就当温习了哈哈,不知道可以收藏点赞以备不时只需 哇额啊（〜^㉨^)〜**