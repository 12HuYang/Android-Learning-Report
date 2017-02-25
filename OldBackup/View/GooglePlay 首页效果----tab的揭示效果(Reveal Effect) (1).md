# GooglePlay 首页效果----tab的揭示效果(Reveal Effect) (1)

> 前言:
> 无意打开GooglePlay app来着,然后发现首页用了揭示效果,连起来用着感觉还不错.
> 不清楚什么是**揭示效果(Reveal Effect)**的效果可以看我前面一篇文章:[Material Design Reveal effect(揭示效果) 你可能见过但是叫不出名字的小效果](http://www.jianshu.com/p/c373011b1784)
> 无法使用Google的小伙伴可以看我更前面的文章:[提高(Android)开发效率的工具与网站](http://www.jianshu.com/p/5f4db061b6b8)

# 工具与分析
- GooglePlay App,这个自己安装好吧,
- 工具 **uiautomatorviewer.bat**,ui分析的工具,在Android sdk的tools目录,建议鼠标右键给它在桌面建个快捷方式,下次桌面双击就可以使用了,非常方便.

# 要点:
- 揭示动画 **ViewAnimationUtils.createCircularReveal()**
- TabLayout 的使用(修改)
- 获取view点击的坐标(用于做动画)

# 效果展示(动画的颜色是随机的)
> demo地址: [https://github.com/didikee/Demos](https://github.com/didikee/Demos)
![效果展示](http://oidz2h9ht.bkt.clouddn.com//blog/2016/12/24/googleplay-main-2016-12-24-154619.gif)

> 墙内的小伙伴可以试试 **APKPure**,一个墙内可以使用,资源是GooglePlay的,缺点:下载慢,优点:能下....(无言以对...)
![Google All](http://oidz2h9ht.bkt.clouddn.com//blog/2016/12/24/google-all-2016-12-24-155431.png)

# 界面布局:
```
<LinearLayout
    android:id="@+id/activity_google_play_tab_reveal"
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.didikee.demos.ui.act.viewActivity.GooglePlayTabRevealActivity"
    >

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="140dp">

        <FrameLayout
            android:id="@+id/below"
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <View
                android:id="@+id/act_view"
                android:layout_width="match_parent"
                android:layout_height="match_parent"/>
        </FrameLayout>

        <com.didikee.demos.ui.tab.ExtTabLayout
            android:id="@+id/sliding_tabs"
            android:layout_width="match_parent"
            android:layout_height="48dp"
            android:layout_gravity="bottom"
            app:tabGravity="fill"
            app:tabIndicatorHeight="1dp"
            app:tabMode="fixed"
            app:tabSelectedTextColor="@color/colorAccent"
            app:tabTextColor="@color/white"/>

    </FrameLayout>

    <android.support.v4.view.ViewPager
        android:id="@+id/viewpager"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/bisque"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"
        />
</LinearLayout>
```

重点是FrameLayout和它内部的子View.
```
<FrameLayout
            android:id="@+id/below"
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <View
                android:id="@+id/act_view"
                android:layout_width="match_parent"
                android:layout_height="match_parent"/>
        </FrameLayout>
```
**子view负责执行动画,FrameLayout负责在子view执行完动画后变成子view执行动画的颜色,这样就能达到一直无限切换的假象**
这个模式和GooglePlay保持了一致,你可以打开**uiautomatorviewer**看看Google的布局.

# java实现
> 这里有个要说明的细节,可能细心的同学会发现,执行动画的起始位置和你手指点击的位置有关,这里的实现的以手指**抬起**的坐标为执行动画的起点.
> GooglePlay的tab不知道是什么写的,我个人感觉不是TabLayout,因为tabLayout默认点击是会有涟漪效果的,但是GooglePlay的tab点击了却没有,我也不想纠结它是用什么做的,我比较偏好TabLayout,所以我就用TabLayout去实现.

## 插一句,关于Android自定义View的三种方式o((≧▽≦o)
**1. 继承 ViewGroup**
**2. 继承 View**
**3. 拷贝源码,然后改改...(～ o ～)~zZ**
今天,用第三种................

# 获取TabLayout点击tab时的坐标:

拷贝Tablayout源码到自己的工程(不要学我....)

TabLayout的组成:

> TabLayout
> > TabView //每个item元素view
> > SlidingTabStrip //每个item下有一条线,也是一个view

我的目标是 **TabView**,在tabView **Selected** 的时候将(`即调用 mTab.select();`)坐标也一起传出去.于是我添加一个接口.

```
 	//--------------add listener
    public interface LocationListener{
        void location(float x,float y,int tabHeight);
    }

    private MyExtTabLayout.LocationListener locationListener;

    public void setLocationListener(MyExtTabLayout.LocationListener locationListener) {
        this.locationListener = locationListener;
    }
    //------------add listener end
```

在tab选中时传出坐标:

```
		@Override
        public boolean onTouchEvent(MotionEvent event) {
            if (event.getAction()==MotionEvent.ACTION_UP){
                x=event.getRawX();
                y=event.getRawY();
            }
            return super.onTouchEvent(event);
        }
        
		@Override
        public boolean performClick() {
            final boolean value = super.performClick();

            if (mTab != null) {
                if (locationListener!=null)locationListener.location(x,y,getHeight());
                mTab.select();
                return true;
            } else {
                return value;
            }
        }
```

# 执行动画
有了坐标就可以执行动画了,动画比较简单.和之前的一篇没多大区别,只是这次我改了执行动画的起始坐标,变成了动态的了.

完整代码:
```
private ViewPager viewPager;
    private SimpleFragmentPagerAdapter pagerAdapter1;

    private ExtTabLayout tabLayout;
    private View viewAnimate;
    private View below;
    private float x;
    private float y;
    private int height;

    private int belowColor;
    private int systemStatusBarHeight;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_google_play_tab_reveal);

        setBarStyle();

        below = findViewById(R.id.below);
        viewAnimate = findViewById(R.id.act_view);

        systemStatusBarHeight = DisplayUtil.getSystemStatusBarHeight(this);

        pagerAdapter1 = new SimpleFragmentPagerAdapter(getSupportFragmentManager(), new
                String[]{"tab1", "tab2", "tab3", "tab4"});
        viewPager = (ViewPager) findViewById(R.id.viewpager);
        tabLayout = (ExtTabLayout) findViewById(R.id.sliding_tabs);
        tabLayout.setupWithViewPager(viewPager);
        tabLayout.setTabMode(ExtTabLayout.MODE_FIXED);

        viewPager.setAdapter(pagerAdapter1);

        belowColor = Color.parseColor(ColorUtil.random());
        below.setBackgroundColor(belowColor);
        viewAnimate.setBackgroundColor(belowColor);

        tabLayout.setLocationListener(new ExtTabLayout.LocationListener() {
            @Override
            public void location(float x, float y, int tabHeight) {
                GooglePlayTabRevealActivity.this.x = x;
                GooglePlayTabRevealActivity.this.y = y;
                GooglePlayTabRevealActivity.this.height = tabHeight;
            }
        });

        tabLayout.addOnTabSelectedListener(new ExtTabLayout.OnTabSelectedListener() {
            @RequiresApi(api = Build.VERSION_CODES.LOLLIPOP)
            @Override
            public void onTabSelected(ExtTabLayout.Tab tab) {
                Log.e("test", "x: " + x + "      y: " + y + "    height: " + height);
                final int width = viewAnimate.getWidth();
                final int height = viewAnimate.getHeight();
                final double radio = Math.sqrt(Math.pow(width, 2) + Math.pow(height, 2));
                float centerX = x;
                float centerY = y;
                Animator circularReveal = ViewAnimationUtils.createCircularReveal(viewAnimate,
                        (int) centerX,
                        (int) centerY, 0, (float) radio);
                circularReveal.setInterpolator(new AccelerateInterpolator());
                circularReveal.setDuration(375);
                circularReveal.addListener(new Animator.AnimatorListener() {
                    @Override
                    public void onAnimationStart(Animator animation) {
                        belowColor = Color.parseColor(ColorUtil.random());
                        viewAnimate.setBackgroundColor(belowColor);
                    }

                    @Override
                    public void onAnimationEnd(Animator animation) {
                        below.setBackgroundColor(belowColor);
                    }

                    @Override
                    public void onAnimationCancel(Animator animation) {

                    }

                    @Override
                    public void onAnimationRepeat(Animator animation) {

                    }
                });
                circularReveal.start();

            }

            @Override
            public void onTabUnselected(ExtTabLayout.Tab tab) {

            }

            @Override
            public void onTabReselected(ExtTabLayout.Tab tab) {

            }
        });

    }

    public void setBarStyle() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
            // 设置状态栏透明
            getWindow().addFlags(WindowManager.LayoutParams.FLAG_TRANSLUCENT_STATUS);
        }
    }

    public class SimpleFragmentPagerAdapter extends FragmentPagerAdapter {
        private String tabTitles[];
        public SimpleFragmentPagerAdapter(FragmentManager fm, String[] strings) {
            super(fm);
            tabTitles = strings;
        }

        @Override
        public Fragment getItem(int position) {
            return PageFragment.newInstance(position + 1);
        }

        @Override
        public int getCount() {
            return tabTitles.length;
        }

        @Override
        public CharSequence getPageTitle(int position) {
            return tabTitles[position];
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        viewAnimate.clearAnimation();
    }
```

> 最后,demo在我的github上,地址是: [https://github.com/didikee/Demos](https://github.com/didikee/Demos)
> 喜欢的点个赞啦





