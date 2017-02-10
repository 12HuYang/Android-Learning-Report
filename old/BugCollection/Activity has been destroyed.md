## java.lang.IllegalStateException: Activity has been destroyed ##

**现在有一个购物车AActivity,它的启动模式是 `SingleTask`,里面填充了一个Fragment**,**当从AActivity-->otherActivity -->AActivity时会出现`crash`:**

**`java.lang.IllegalStateException: Activity has been destroyed`**

**解决的代码如下:**

	public class ShopCartActivity extends BaseActivity
	{

	private boolean isInit=false;
	@Override
	protected void onCreate(Bundle savedInstanceState)
	{
		super.onCreate(savedInstanceState);
		setContentView(R.layout.view_container);
		init();
	}

	private void init()
	{
		getSDFragmentManager().replace(R.id.view_container_fl_content, new ShopCartFragment());
	}

	@Override
	protected void onNewIntent(Intent intent)
	{
		super.onNewIntent(intent);
		//onNewIntent比onRestart先执行;
		Log.e("wwh", "onNewIntent");
		isInit=true;
		init();
	}
	
	@Override
	protected void onRestart() {
		super.onRestart();
		Log.e("wwh", "onRestart");
		if (!isInit) {
			init();
		}
	  }
	}

**当执行完`onNewIntent()`后执行 `OnRestart()` ,`init()`方法走了`两遍`,Fragment的`replace()`方法走了两遍.进入方法里面查看,出错在这里:**

	public Fragment replace(int container, Fragment fragment, Bundle args, boolean addToBackStack)
	{
		if (fragment != null)
		{
			putFragmentData(fragment, args);
			final FragmentTransaction transaction = beginTransaction();
			final String tag = fragment.getClass().getSimpleName();

			transaction.replace(container, fragment, tag);

			if (addToBackStack)
			{
				transaction.addToBackStack(null);
			}
			//这句报错
			transaction.commitAllowingStateLoss();
		}
		return fragment;
	}

**事务在commit的时候crash了,`transaction.commitAllowingStateLoss();`**

在StackOverFlow上有人贴出了一个不错的外文blog:
[Fragment-transation-commit的时机](http://www.androiddesignpatterns.com/2013/08/fragment-transaction-commit-state-loss.html),里面有一段写到:

**`Be careful when committing transactions inside Activity lifecycle methods.`** A large majority of applications will only ever commit transactions the very first time onCreate() is called and/or in response to user input, and will never face any problems as a result. However, as your transactions begin to venture out into the other Activity lifecycle methods, such as onActivityResult(), onStart(), and onResume(), things can get a little tricky. For example, you should not commit transactions inside the FragmentActivity#onResume() method, as there are some cases in which the method can be called before the activity’s state has been restored (see the documentation for more information). If your application requires committing a transaction in an Activity lifecycle method other than onCreate(), do it in either FragmentActivity#onResumeFragments() or Activity#onPostResume(). These two methods are guaranteed to be called after the Activity has been restored to its original state, and therefore avoid the possibility of state loss all together. (As an example of how this can be done, check out my answer to this StackOverflow question for some ideas on how to commit FragmentTransactions in response to calls made to the Activity#onActivityResult() method).