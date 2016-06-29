## Android 在View中更新View ##

直接用**`Invalidate()`**方法会导致错误:**只有主线程才能更新UI**

取而代之的是可以使用**`postInvalidate()`**;

#### 原因: ####

最终会调用**`ViewRootImpl`**类的**`dispatchInvalidateDelayed(View view, long delayMilliseconds) `**方法;

代码如下:

	public void dispatchInvalidateDelayed(View view, long delayMilliseconds) {
        Message msg = mHandler.obtainMessage(MSG_INVALIDATE, view);
        mHandler.sendMessageDelayed(msg, delayMilliseconds);
		//看到Handler就不用说为什么它能更新ui了吧
    }

在**`postInvalidate()`**方法:

	public void postInvalidateDelayed(long delayMilliseconds) {
        // We try only with the AttachInfo because there's no point in invalidating
        // if we are not attached to our window
        final AttachInfo attachInfo = mAttachInfo;
        if (attachInfo != null) {
            attachInfo.mViewRootImpl.dispatchInvalidateDelayed(this, delayMilliseconds);
        }
    }

   
    public void postInvalidateDelayed(long delayMilliseconds, int left, int top,
            int right, int bottom) {

        // We try only with the AttachInfo because there's no point in invalidating
        // if we are not attached to our window
        final AttachInfo attachInfo = mAttachInfo;
        if (attachInfo != null) {
            final AttachInfo.InvalidateInfo info = AttachInfo.InvalidateInfo.obtain();
            info.target = this;
            info.left = left;
            info.top = top;
            info.right = right;
            info.bottom = bottom;

			//这个最终调用ViewRootImpl类的dispatchInvalidateRectDelayed(info, delayMilliseconds)方法;
            attachInfo.mViewRootImpl.dispatchInvalidateRectDelayed(info, delayMilliseconds);
        }
    }