# Android 启动模式--任务(Task)--桟 的误区

> 写这篇文章是因为前几天的一次面试,面试官说`SingleInstance`模式会新建一个桟,而`SingleTask`不会.首先不说这个对不对(非要说对错的话,那就是错.),因为这句话是**含糊不清**的.**桟**?只的是**返回桟**? 还是**任务桟**?有没有考虑`taskAffinity`属性?所以笼统的那样说是不对的.这篇文章一是为了记录,二是为了说清楚----任务(Task)& 桟(返回桟,任务桟).

## 概念

**桟(堆栈:stack)**

>栈的基本特点：
- 先入后出，后入先出。
- 除头尾节点之外，每个元素有一个前驱，一个后继。

**任务**

>**任务**是指在执行特定作业时与用户交互的一系列 Activity。 这些 Activity 按照各自的打开顺序排列在堆栈（即返回栈）中。

**返回桟**
在官方文档中,找不到关于返回桟的概念,但是按照官方文档在描述时的语境,可以理解为**响应Android返回键的顺序队列.**这是站在用户的直观感受上的描述.Android 内部的描述单位只有**任务(Task)**.

**任务桟**
在官方文档中没有这个概念.我为了方便理解,把`任务`中按照`桟`规则的顺序队列的 一系列 Activity 称为 `任务栈`.

### 个人理解

由于 官方文档中没有详细的说明`什么是返回桟`,在实际的开发中如果以栈为基本`度量单位`的话,很容易被自己绕晕,如果按照`任务`为基本度量单位的话就很容易理清楚 `Android 四大启动模式`了.我个人把返回栈理解为这样子的: **返回桟仅有一个(因为返回键只有一个),返回桟操作的基本元素是`任务`,返回键操作的是返回桟中处于前台的`任务`,每个`任务`维护着一系列 Activity 组成的`栈`以响应返回键.**

## Activity 四大启动模式

- "standard"（默认模式）
**默认。**系统在启动 Activity 的任务中创建 Activity 的新实例并向其传送 Intent。Activity 可以多次实例化，而每个实例均可属于不同的任务，并且一个任务可以拥有多个实例。
- "singleTop"
如果当前任务的顶部已存在 Activity 的一个实例，则系统会通过调用该实例的 onNewIntent() 方法向其传送 Intent，而不是创建 Activity 的新实例。Activity 可以多次实例化，而每个实例均可属于不同的任务，并且一个任务可以拥有多个实例（但前提是位于返回栈顶部的 Activity 并不是 Activity 的现有实例）。
例如，假设任务的返回栈包含根 Activity A 以及 Activity B、C 和位于顶部的 D（堆栈是 A-B-C-D；D 位于顶部）。收到针对 D 类 Activity 的 Intent。如果 D 具有默认的 "standard" 启动模式，则会启动该类的新实例，且堆栈会变成 A-B-C-D-D。但是，如果 D 的启动模式是"singleTop"，则 D 的现有实例会通过 onNewIntent() 接收 Intent，因为它位于堆栈的顶部；而堆栈仍为 A-B-C-D。但是，如果收到针对 B 类 Activity 的 Intent，则会向堆栈添加 B 的新实例，即便其启动模式为 "singleTop" 也是如此。
> 注：为某个 Activity 创建新实例时，用户可以按“返回”按钮返回到前一个 Activity。 但是，当 Activity 的现有实例处理新 Intent 时，则在新 Intent 到达 onNewIntent() 之前，用户无法按“返回”按钮返回到 Activity 的状态。

- "singleTask"
**系统创建新任务并实例化位于新任务底部的 Activity**。但是，如果该 Activity 的一个实例已存在于一个单独的任务中，则系统会通过调用现有实例的 onNewIntent() 方法向其传送 Intent，而不是创建新实例。一次只能存在 Activity 的一个实例。
注：尽管 Activity 在新任务中启动，但是用户按“返回”按钮仍会返回到前一个 Activity。

- "singleInstance".
**与 "singleTask" 相同，只是系统不会将任何其他 Activity 启动到包含实例的任务中。该 Activity 始终是其任务唯一仅有的成员；**由此 Activity 启动的任何 Activity 均在单独的任务中打开。

我们再来看另一示例，Android 浏览器应用声明网络浏览器 Activity 应始终在其自己的任务中打开（通过在 <activity> 元素中指定 singleTask 启动模式）。这意味着，如果您的应用发出打开 Android 浏览器的 Intent，则其 Activity 与您的应用位于不同的任务中。相反，系统会为浏览器启动新任务，或者如果浏览器已有任务正在后台运行，则会将该任务上移一层以处理新 Intent。

![](pic/diagram_backstack_singletask_multiactivity.png)
无论 Activity 是在新任务中启动，还是在与启动 Activity 相同的任务中启动，用户按“返回”按钮始终会转到前一个 Activity。 但是，如果启动指定 singleTask 启动模式的 Activity，则当某后台任务中存在该 Activity 的实例时，整个任务都会转移到前台。此时，返回栈包括上移到堆栈顶部的任务中的所有 Activity。 上图显示了这种情况。


上图 显示如何将启动模式为“singleTask”的 Activity 添加到返回栈。 如果 Activity 已经是某个拥有自己的返回栈的后台任务的一部分，则整个返回栈也会上移到当前任务的顶部。

> 参考:[2017-03-14日摘自官方文档(后期可能有变更以官网为准)](https://developer.android.google.cn/guide/components/tasks-and-back-stack.html)


## 关于`android:taskAffinity`属性
> ### android:taskAffinity
与 Activity 有着亲和关系的任务。从概念上讲，具有相同亲和关系的 Activity 归属同一任务（从用户的角度来看，则是归属同一“应用”）。 任务的亲和关系由其根 Activity 的亲和关系确定。
亲和关系确定两件事 - Activity 更改到的父项任务（请参阅 allowTaskReparenting 属性）和通过 FLAG_ACTIVITY_NEW_TASK 标志启动 Activity 时将用来容纳它的任务。
默认情况下，应用中的所有 Activity 都具有相同的亲和关系。您可以设置该属性来以不同方式组合它们，甚至可以将在不同应用中定义的 Activity 置于同一任务内。 要指定 Activity 与任何任务均无亲和关系，请将其设置为空字符串。
如果未设置该属性，则 Activity 继承为应用设置的亲和关系（请参阅 <application> 元素的 taskAffinity 属性）。 应用默认亲和关系的名称是 <manifest> 元素设置的软件包名称。

> 参考:[2017-03-14日摘自官方文档(后期可能有变更以官网为准)](https://developer.android.google.cn/guide/topics/manifest/activity-element.html#aff)


## 测试 `SingleTask`与`SingleInstance`的异同
我写了一个Demo测试了下:

当我把`SingleTaskA`配置为:
```
<activity android:name=".SingleTaskA" android:launchMode="singleTask" android:taskAffinity="com.didikee.temp"/>
```
在`SingleTaskA`中启动一个标准模式的`StandardB`Activity:
```
public class SingleTaskA extends BaseLauncherModeActivity {
    @Override
    protected Class gotoActivity() {
        return StandardB.class;
    }

    @Override
    protected String setModeTextShow() {
        return Constant.SINGLE_TASK;
    }
}
```
而在`StandardB`中,我一直启动`StandardB`自己:
```
public class StandardB extends BaseLauncherModeActivity {
    @Override
    protected Class gotoActivity() {
        return StandardB.class;
    }

    @Override
    protected String setModeTextShow() {
        return Constant.STANDARD+"Repeat";
    }
}
```

补充下基类:
```
public abstract class BaseLauncherModeActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_base_launcher_mode);
        String text = setModeTextShow();
        Button button = (Button) findViewById(R.id.bt);
        button.setText("模式: "+text);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Class aClass = gotoActivity();
                if (aClass==null)return;
                startActivity(new Intent(getApplicationContext(),aClass));
            }
        });
        int taskId = getTaskId();
        Log.d(Constant.TAG,"taskId: "+taskId);
    }

    protected abstract Class gotoActivity();

    protected abstract String setModeTextShow();
}
```
得到如下结果:
```
03-14 10:32:32.287 22561-22561/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 26
03-14 10:32:37.691 22561-22561/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 26
03-14 10:32:42.120 22561-22561/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 27
03-14 10:32:45.961 22561-22561/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 27
03-14 10:32:52.105 22561-22561/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 27
03-14 10:32:53.531 22561-22561/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 27
03-14 10:32:55.748 22561-22561/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 27
03-14 10:32:56.763 22561-22561/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 27
```
**可以看到,`SingleTaskA`启动了一个新的`任务(Task)`,id为27,之后在SingleTaskA中启动`StandardB`并没有做其他的操作,而是直接在刚刚`SingleTaskA`启动的`任务`中添加.**

**现在我去掉 `taskAffinity`属性的定义**,得到:
```
03-14 16:42:10.604 31062-31062/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 29
03-14 16:42:15.261 31062-31062/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 29
03-14 16:42:16.865 31062-31062/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 29
03-14 16:42:19.197 31062-31062/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 29
03-14 16:42:20.232 31062-31062/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 29
03-14 16:42:21.049 31062-31062/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 29
03-14 16:42:22.180 31062-31062/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 29
```
**可见,默认是不会创建新的`任务`的.**

**现在,把SingleTaskA 改为 SingleInstanceA,然后在其中可以启动`StandardB`(上面有代码),得到如下结果:**

```
03-14 16:46:59.669 2273-2273/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 30
03-14 16:47:09.367 2273-2273/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 30
03-14 16:47:10.602 2273-2273/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 31
03-14 16:47:12.013 2273-2273/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 30
03-14 16:47:14.438 2273-2273/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 30
03-14 16:47:15.429 2273-2273/com.didikee.androidlaunchermode D/AndroidLauncherMode: taskId: 30
```
**可见,`SingleInstanceA`新建了一个`任务`id为31,在 SingleInstanceA 中启动 StandardB 还是继续添加在之前`任务`中.**

## 总结:
1. Android 是以`任务`为核心的,不要被栈带偏了.
2. `SingleInstance` **一定会**新建一个`任务`,**并且该`任务`中仅包含一个实例.**
3. `SingleTask`默认不会创建新`任务`,但是可以通过`taskAffinity`达到创建新`任务`的目的,创建的`任务`可添加其他元素.

> 最后,说的不对的欢迎交流.