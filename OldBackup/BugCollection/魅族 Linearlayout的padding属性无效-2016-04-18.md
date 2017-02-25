## 魅族 Linearlayout的padding属性无效?! ##

	//这是更改之后的...(margin生效)
	<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="wrap_content"
	    android:background="@drawable/cricle_white"
	     >
	
	    <LinearLayout
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:layout_margin="15dp"
	        android:orientation="vertical" >
		//.......


	//android:padding="15dp"不生效了...
	<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="wrap_content"
	    android:background="@drawable/cricle_white"
	     >
	
	    <LinearLayout
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:padding="15dp"
	        android:orientation="vertical" >
		//.......
无语啊.....