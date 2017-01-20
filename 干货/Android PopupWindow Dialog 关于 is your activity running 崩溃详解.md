# Android PopupWindow Dialog 关于 is your activity running 崩溃详解
[TOC]

## 起因

**对于 `PopupWindow Dialog` 需要 Activity 作为容器,并于其生命周期联系在一起.在Activity 还没有初始化完成时,此时我们调用 `PopupWindow Dialog` 的`show()`方法就会抛出异常:**
```
throw new WindowManager.BadTokenException("Unable to add window -- token " + attrs.token+ " is not valid; is your activity running?");
```

常见的崩溃日志如下:
```
android.view.WindowManager$BadTokenException: Unable to add window -- token android.os.BinderProxy@406a074 is not valid; is your activity running?
at android.view.ViewRoot.setView(ViewRoot.java:530)
at android.view.WindowManagerImpl.addView(WindowManagerImpl.java:199)
at android.view.WindowManagerImpl.addView(WindowManagerImpl.java:113)
at android.view.Window$LocalWindowManager.addView(Window.java:424)
at android.app.Dialog.show(Dialog.java:241)
at com.eleybourn.bookcatalogue.dialogs.StandardDialogs.goodreadsAuthAlert(StandardDialogs.java:261)
at com.eleybourn.bookcatalogue.goodreads.GoodreadsUtils$4$1.run(GoodreadsUtils.java:101)
at android.os.Handler.handleCallback(Handler.java:587)
at android.os.Handler.dispatchMessage(Handler.java:92)
at android.os.Looper.loop(Looper.java:130)
at android.app.ActivityThread.main(ActivityThread.java:3687)
at java.lang.reflect.Method.invokeNative(Native Method)
at java.lang.reflect.Method.invoke(Method.java:507)
at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:878)
at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:636)
at dalvik.system.NativeStart.main(Native Method)
```

## 解决办法

```
if(!Activity.isFinishing()){
	mPopupWindow.show(anchor);
}
```

**如果页面结束时 `PopupWindow Dialog` 没有`dismiss()`,那么会出现内存泄漏,日志如下:**
```
SpecialTopicActivity has leaked window android.widget.PopupWindow$PopupDecorView{dfa91cc V.E...... ........ 0,0-185,86} that was originally added here
at android.view.ViewRootImpl.<init>(ViewRootImpl.java:573)
at android.view.WindowManagerGlobal.addView(WindowManagerGlobal.java:326)
at android.view.WindowManagerImpl.addView(WindowManagerImpl.java:109)
at android.widget.PopupWindow.invokePopup(PopupWindow.java:1333)
at android.widget.PopupWindow.showAsDropDown(PopupWindow.java:1156)
at android.widget.PopupWindow.showAsDropDown(PopupWindow.java:1115)
at com.fanwe.customview.PopTipShare.show(PopTipShare.java:56)
at com.fanwe.seller.views.SpecialTopicActivity$4.run(SpecialTopicActivity.java:222)
at android.os.Handler.handleCallback(Handler.java:739)
at android.os.Handler.dispatchMessage(Handler.java:95)
at android.os.Looper.loop(Looper.java:148)
at android.app.ActivityThread.main(ActivityThread.java:7224)
at java.lang.reflect.Method.invoke(Native Method)
at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:1230)
at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1120)
...
```

在Activity执行`onDestroy()`时 dismiss 掉 `PopupWindow Dialog`即可;

```
if (mPopupWindow!=null && mPopupWindow.isShowing()){
            mPopupWindow.dismiss();
  }
```

## 源码
需要涉及的类:
- `ViewManager` : `WindowManager` 的父类.
- `WindowManager` 及其实现类 `WindowManagerImpl(@hide)`: 接口类与实现类,对用户开放.
- `ViewRootImpl(@hide)` : WindowManager 的 View 的操作实现类.
- `WindowManagerGlobal(@hide)` :`WindowManagerImpl` 类功能的执行者.

**1. 在调用 Show() 方法时**
注:以下情况都是对`PopupWindow Dialog` 适用的,现在不再指明`PopupWindow Dialog`,下面以`PopupWindow`为例一步步说明.
**直接展示源码可能更容易说明问题,注释是关键点,以下源码展示以方法执行顺序进行.**
```
public void showAsDropDown(View anchor, int xoff, int yoff, int gravity) {
        if (isShowing() || mContentView == null) {
            return;
        }

        TransitionManager.endTransitions(mDecorView);
        attachToAnchor(anchor, xoff, yoff, gravity);
        mIsShowing = true;
        mIsDropdown = true;
        final WindowManager.LayoutParams p = createPopupLayoutParams(anchor.getWindowToken());
        preparePopup(p);

        final boolean aboveAnchor = findDropDownPosition(anchor, p, xoff, yoff,
                p.width, p.height, gravity);
        updateAboveAnchor(aboveAnchor);
        p.accessibilityIdOfAnchor = (anchor != null) ? anchor.getAccessibilityViewId() : -1;
		
        //以上都是准备 WindowManager.LayoutParams p;
        invokePopup(p);
    }
```
```
private void invokePopup(WindowManager.LayoutParams p) {
        if (mContext != null) {
            p.packageName = mContext.getPackageName();
        }

        final PopupDecorView decorView = mDecorView;
        decorView.setFitsSystemWindows(mLayoutInsetDecor);

        setLayoutDirectionFromAnchor();

		//调用WindowManager.addView()方法
        mWindowManager.addView(decorView, p);

        if (mEnterTransition != null) {
            decorView.requestEnterTransition(mEnterTransition);
        }
    }
```
以上都是在`PopupWindow`,在调用`WindowManager.addView()`后进入`WindowManager`类,实际是其实现类`WindowManagerImpl`.
```
    @Override
    public void addView(@NonNull View view, @NonNull ViewGroup.LayoutParams params) {
        applyDefaultToken(params);
        //调用了 WindowManagerGlobal.addView()方法.
        mGlobal.addView(view, params, mContext.getDisplay(), mParentWindow);
    }
```
**重点在这里 WindowManagerGlobal.addView()**
```
    public void addView(View view, ViewGroup.LayoutParams params,
            Display display, Window parentWindow) {
        // ...
        // 检查一些状态
        // ViewRootImpl root : 添加 View 最后一步由此 View 操作类的 setView() 完成.
        ViewRootImpl root;
        View panelParentView = null;

        	//...
        	//准备需要的参数
            root = new ViewRootImpl(view.getContext(), display);
            view.setLayoutParams(wparams);
            mViews.add(view);
            mRoots.add(root);
            mParams.add(wparams);
        }

        // do this last because it fires off messages to start doing things
        try {
        	//ViewRootImpl类的 setView() 方法.
            root.setView(view, wparams, panelParentView);
        } catch (RuntimeException e) {
        	// 异常在这里抛出
            // BadTokenException or InvalidDisplayException, clean up.
            synchronized (mLock) {
                final int index = findViewLocked(view, false);
                if (index >= 0) {
                    removeViewLocked(index, true);
                }
            }
            throw e;
        }
    }
```
**最后进入 ViewRootImpl 的 setView() 方法**
```

    /**
     * We have one child
     */
    public void setView(View view, WindowManager.LayoutParams attrs, View panelParentView) {
        synchronized (this) {
            if (mView == null) {
                mView = view;

                mAttachInfo.mDisplayState = mDisplay.getState();
                mDisplayManager.registerDisplayListener(mDisplayListener, mHandler);

                mViewLayoutDirectionInitial = mView.getRawLayoutDirection();
                //最终把 View 添加上去,如果异常了就传null.
                mFallbackEventHandler.setView(view);
                mWindowAttributes.copyFrom(attrs);
                //...省略代码
                if (DEBUG_LAYOUT) Log.v(mTag, "Added window " + mWindow);
                if (res < WindowManagerGlobal.ADD_OKAY) {
                    mAttachInfo.mRootView = null;
                    mAdded = false;
                    mFallbackEventHandler.setView(null);
                    unscheduleTraversals();
                    setAccessibilityFocus(null, null);
                    switch (res) {
                        case WindowManagerGlobal.ADD_BAD_APP_TOKEN:
                        case WindowManagerGlobal.ADD_BAD_SUBWINDOW_TOKEN:
                        //这里集中处理异常情况
                        //异常实际抛出的ADD_BAD_SUBWINDOW_TOKEN
                            throw new WindowManager.BadTokenException(
                                    "Unable to add window -- token " + attrs.token
                                    + " is not valid; is your activity running?");
                        case WindowManagerGlobal.ADD_NOT_APP_TOKEN:
                            throw new WindowManager.BadTokenException(
                                    "Unable to add window -- token " + attrs.token
                                    + " is not for an application");
                        case WindowManagerGlobal.ADD_APP_EXITING:
                            throw new WindowManager.BadTokenException(
                                    "Unable to add window -- app for token " + attrs.token
                                    + " is exiting");
                        case WindowManagerGlobal.ADD_DUPLICATE_ADD:
                            throw new WindowManager.BadTokenException(
                                    "Unable to add window -- window " + mWindow
                                    + " has already been added");
                        case WindowManagerGlobal.ADD_STARTING_NOT_NEEDED:
                            // Silently ignore -- we would have just removed it
                            // right away, anyway.
                            return;
                        case WindowManagerGlobal.ADD_MULTIPLE_SINGLETON:
                            throw new WindowManager.BadTokenException("Unable to add window "
                                    + mWindow + " -- another window of type "
                                    + mWindowAttributes.type + " already exists");
                        case WindowManagerGlobal.ADD_PERMISSION_DENIED:
                            throw new WindowManager.BadTokenException("Unable to add window "
                                    + mWindow + " -- permission denied for window type "
                                    + mWindowAttributes.type);
                        case WindowManagerGlobal.ADD_INVALID_DISPLAY:
                            throw new WindowManager.InvalidDisplayException("Unable to add window "
                                    + mWindow + " -- the specified display can not be found");
                        case WindowManagerGlobal.ADD_INVALID_TYPE:
                            throw new WindowManager.InvalidDisplayException("Unable to add window "
                                    + mWindow + " -- the specified window type "
                                    + mWindowAttributes.type + " is not valid");
                    }
                    throw new RuntimeException(
                            "Unable to add window -- unknown error code " + res);
                }
				//....省略代码
            }
        }
     }
```
从上面的代码中可以看出,判断异常类型的是一个`int`值res,现在看看res.
```
try {
     mOrigWindowType = mWindowAttributes.type;
     mAttachInfo.mRecomputeGlobalAttributes = true;
     collectViewAttributes();
     // mWindowSession 的类型是 IWindowSession , mWindow 的类型是 IWindow.Stub .这行代码就是利用AIDL进行IPC, 实际被调用的是Session.addToDisplay()方法.
     res = mWindowSession.addToDisplay(mWindow, mSeq, mWindowAttributes,
           getHostVisibility(), mDisplay.getDisplayId(),
           mAttachInfo.mContentInsets, mAttachInfo.mStableInsets,
           mAttachInfo.mOutsets, mInputChannel);
                } catch {//...}
```
进一步调用Session.java中的addToDisplay方法:
```
    @Override
    public int addToDisplay(IWindow window, int seq, WindowManager.LayoutParams attrs,
            int viewVisibility, int displayId, Rect outContentInsets, Rect outStableInsets,
            Rect outOutsets, InputChannel outInputChannel) {
        return mService.addWindow(this, window, seq, attrs, viewVisibility, displayId,
                outContentInsets, outStableInsets, outOutsets, outInputChannel);
    }
```
mService是WindowManagerService,继续看 WmS 的 addWindow() 方法.
**这是最核心的类,关于所有的Android addView 最后都是通过 WmS 的 `addWindow()`方法完成添加操作.**
```
public int addWindow(Session session, IWindow client, int seq,
            WindowManager.LayoutParams attrs, int viewVisibility, int displayId,
            Rect outContentInsets, Rect outStableInsets, Rect outOutsets,
            InputChannel outInputChannel) {
        int[] appOp = new int[1];
        
        // 关于权限的检查,如果有使用 WindowManager 实现悬浮窗效果的对悬浮窗,关于 SYSTEM_ALERT_WINDOW 的申请问题肯定纠结过.
        // 我在一篇文章说对于SDK 19以上,使用 WindowManager.LayoutParams.TYPE_TOAST,SDK 19 以下使用 WindowManager.LayoutParams.TYPE_PHONE
        // 原因是:1.type为"TYPE_TOAST"在sdk19之前不接收事件,之后可以.
        //		 2.type为"TYPE_PHONE"需要"SYSTEM_ALERT_WINDOW"权限.在sdk19之前不可以直接申明使用,之后不能直接申明使用.
        // 想知道为什么会这样,可以看看 mPolicy.checkAddPermission(attrs, appOp); 答案在这里面.(这里就不看了=.=)
        int res = mPolicy.checkAddPermission(attrs, appOp);
        if (res != WindowManagerGlobal.ADD_OKAY) {
            return res;//如果权限不满足就不用继续了.
        }

        boolean reportNewConfig = false;
        WindowState attachedWindow = null;
        long origId;
        final int type = attrs.type;

        //...
        //check something...

            boolean addToken = false;
            
            // mTokenMap 存储 WindowToken;
            // 这里是取,后面在会执行 mTokenMap.put()方法,这样 token 就不为null了.
            // Activity 的addWindow时会传入不为 null 的 token,然而 PopupWindow 和 Dialog 传入的是为 null 的token.
    		//final HashMap<IBinder,  WindowToken> mTokenMap = new HashMap<>();
            WindowToken token = mTokenMap.get(attrs.token);
            
            AppWindowToken atoken = null;
            //...
            //check something...

            if (addToken) {
            //如果activity调用 WindowManager.addView(),token就会被 put 到 map 中.
                mTokenMap.put(attrs.token, token);
            }
            win.attach();
            mWindowMap.put(client.asBinder(), win);
            //...省略

        return res;
    }
```
现在应该知道了在哪里抛出异常了,最后还有一点就是关于疑问的: **那 Activity 什么时候 才算 is running ?**

## Activity 什么时候 才算 is running ?
现在看看 **ActivityThread**类,这是 Activity 的管理类,分发Activity的生命周期等.
在 ActivityThread 的 handleResumeActivity() 方法中,有如下代码:

```
final void handleResumeActivity(IBinder token,
            boolean clearHide, boolean isForward, boolean reallyResume, int seq, String reason) {
        //...
        // performResumeActivity() 方法最后会调用 Activity 的 OnResume() 方法.
        r = performResumeActivity(token, clearHide, reason);

			//....
            if (r.window == null && !a.mFinished && willBeVisible) {
                r.window = r.activity.getWindow();
                View decor = r.window.getDecorView();
                decor.setVisibility(View.INVISIBLE);
                ViewManager wm = a.getWindowManager();
                WindowManager.LayoutParams l = r.window.getAttributes();
                a.mDecor = decor;
                l.type = WindowManager.LayoutParams.TYPE_BASE_APPLICATION;
                l.softInputMode |= forwardBit;
                if (r.mPreserveWindow) {
                    a.mWindowAdded = true;
                    r.mPreserveWindow = false;
                    // Normally the ViewRoot sets up callbacks with the Activity
                    // in addView->ViewRootImpl#setView. If we are instead reusing
                    // the decor view we have to notify the view root that the
                    // callbacks may have changed.
                    ViewRootImpl impl = decor.getViewRootImpl();
                    if (impl != null) {
                        impl.notifyChildRebuilt();
                    }
                }
                if (a.mVisibleFromClient && !a.mWindowAdded) {
                    a.mWindowAdded = true;
                    //这里调用了 WindowManager 的 addView() 方法,最终会调用 WindowManagerService 的 addWindow() 方法.然后就是之前看到的源码内容了.其他的情况也类似这个流程,但是还是有很多细微区别,比如 WindowManager.LayoutParams 的 Type 与 Flag 等.
                    wm.addView(decor, l);
                }
			//...
    }
```

在 Activity 的生命周期 onResume 执行后不久, Activity 的 token 随着 `wm.addView(decor, l);` 后就被 put 到 map 中,其后调用 PopupWindow 与 Dialog 的 Show() 方法后就不会出现 "is your activity running ?"这种异常了.由于token在 performResumeActivity() 后(从代码中可以看出,是在同一方法体中执行完两个操作),所以有人在 Activity 的 onResume() 方法中这样写道:

```
    @Override
    protected void onResume() {
        super.onResume();
        mListview.postDelayed(new Runnable() {
            @Override
            public void run() {
                mPopupWindow.show(anchor);
            }
        },100);
    }
```
这种写法是不可取的.

**在实际工作过程中,可能需要在界面展示后2s展示一个Dialog 或者 PopupWindow ,此时如果用户在2s内退出 Activity ,那么Runnable执行时无法使用一个Finished 的Activity 这里推荐的写法.**

```
//第一种
mSomeView.postDelayed(new Runnable() {
                @Override
                public void run() {
                    if (!Activity.this.isFinishing()){
                        mPopupWindow.show();
                    }
                }
            },1000);
//第二种
mSomeView.post(new Runnable() {
                @Override
                public void run() {
                    if (!Activity.this.isFinishing()){
                        mPopupWindow.show();
                    }
                }
            });
```
**[Bugly社区整理一篇关于 Window 的文章总结的很不错,对于 Window 不是很了解的可以点此了解下.](http://bugly.qq.com/bbs/forum.php?mod=viewthread&tid=555)** 我之前整理过PopupWindow 的实现过程,大概有一年了吧,现在竟然忘的差不多了,额....

> 剩余就是 **Activity.isFinishing()** 方法的具体调用与实现过程了. Google API 的说明是: `Check to see whether this activity is in the process of finishing, either because you called finish on it or someone else has requested that it finished. This is often used in onPause to determine whether the activity is simply pausing or completely finishing.` 检测 Activity 是否在 **Process 或者 Finishing**过程中.有空看看具体过程.