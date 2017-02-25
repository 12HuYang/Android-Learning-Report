## 防止控件被快速的doubleClick ##

> 2016/4/7 14:21:20 

	/**
	 * 防止二次点击,代替onclick()方法.
	 * @author didik
	 *
	 */
	public abstract class NoDoubleClickListener implements OnClickListener{
		
		private final int MIN_CLICK_DELAY_TIME=800;
		private long lastClickTime=0;
		@Override
		public void onClick(View v) {
			long currentTime = Calendar.getInstance().getTimeInMillis();
	        if (currentTime - lastClickTime > MIN_CLICK_DELAY_TIME) {
	            lastClickTime = currentTime;
	            onNoDoubleClick(v);
	        } 
		}
		protected abstract void onNoDoubleClick(View v);
	
	}