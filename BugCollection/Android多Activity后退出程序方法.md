##Android多Activity后退出程序方法:

###注:
	1. 网上的关于强行对进程和虚拟机操作的方法全部失效;
	2. 这些方法不例呼 System.exit(0)或者KillProcess();
	3. 2016/3/13 10:37:47 这是本次方法的时间,android版本4.4
###方法:
	多activity中退出整个程序，例如从A->B->C->D，这时我需要从D直接退出程序。

	通过stack的原理来巧妙的实现，在D窗口打开A窗口时在Intent中直接加入标志Intent.FLAG_ACTIVITY_CLEAR_TOP，
	再次开启A时将会清除该进程空间的所有Activity。
	
	在D中使用下面的代码:
	Intent intent = new Intent();
	intent.setClass(D.this, A.class);
	intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);  //注意本行的FLAG设置
	startActivity(intent);
	finish();   
	
	在A中加入代码：
	Override
	
	protected void onNewIntent(Intent intent) {
	// TODO Auto-generated method stub
	
	super.onNewIntent(intent);
	
	//退出
	if ((Intent.FLAG_ACTIVITY_CLEAR_TOP & intent.getFlags()) != 0) {
	               finish();
	   		}
	}
	A的Manifest.xml配置成android:launchMode="singleTop"

###分析原理:
	一般A是程序的入口点，从D起一个A的activity，加入标识Intent.FLAG_ACTIVITY_CLEAR_TOP这个过程中会把栈中B，C，都清理掉。因为A是android:launchMode="singleTop"
	不会调用oncreate(),而是响应onNewIntent（）这时候判断Intent.FLAG_ACTIVITY_CLEAR_TOP，然后把A finish（）掉。
	栈中A,B,C,D全部被清理。所以整个程序退出了。