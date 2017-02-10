## Dialog 的使用汇总(1) ##

#### 项目中一般都是自定义布局的 Dialog,几乎不用原生 Dialog,那么可以写一个util来控制dialog的显示,例如: ####

	/**
	 * 
	 * @param context
	 * @param resLayout
	 * @return 返回一个dialog
	 */
	private static AlertDialog getDialog(Context context,int resLayout){
		View view=View.inflate(context, resLayout, null);
		AlertDialog.Builder builder=new AlertDialog.Builder(context);
		return builder.setView(view).create();
	}

	/**
	 * 展示一个无交互,更多设置选项
	 * 
	 * @param context Activity
	 * @param resLayout 布局
	 * @param alpha dialog外围背景的透明度([0,1.0]:1.0表示全不透明(全黑))
	 * @param cancleOnTouchOutside 是否点击其他区域dismiss dialog
	 */
	public static void showDialog(Context context,int resLayout,float alpha,boolean cancleOnTouchOutside) {
		if (alpha<0 || alpha > 1.0f ) {
			return ;
		}
		AlertDialog dialog = getDialog(context, resLayout);
		dialog.getWindow().setDimAmount(alpha);
		dialog.setCanceledOnTouchOutside(cancleOnTouchOutside);
		dialog.show();
	}

### 控制dialog的弹出位置: ###

**通过Window类的setGravity(int)来进行设置。**

如：在屏幕顶端显示：

	Dialogdialog = dialog = new Dialog(this, R.style.dialog);
	
	dialog.setContentView(R.layout.q006_dialog); //R.layout.q006_dialog为自定义的dilog布局
	
	Windowmwindow = dialog.getWindow();
	
	mwindow.setGravity(Gravity.TOP);
	
	dialog.show();

### 如何在点击位置显示Dialog？ ###

首先，需要获得被点击View的绝对坐标：

	//假设屏幕中有一个ImageView img，获得其在屏幕上的坐标
	
	finalint[] location = new int[2];
	img.getLocationOnScreen(location);
	
	然后，通过设置WindowManager.LayoutParams，控制Dialog显示位置
	
	WinowManager.LayoutParams lp = mwindow.getAttributes();
	lp.x= location[0];
	lp.y= location[1];
	Toast.makeText(context,"x:"+lp.x+",y:"+lp.y, 3000).show();
	mwindow.setAttributes(lp);

### 设置dialog外围background的透明度 ###

	dialog.getWindow().setDimAmount(alpha);
	//alpha 为float类型,值为[0,1.0],1.0表示全不透明(全黑)