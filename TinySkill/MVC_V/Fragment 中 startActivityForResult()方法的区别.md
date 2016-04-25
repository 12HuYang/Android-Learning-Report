## Fragment 中 startActivityForResult()方法的区别: ##

谁调用,走谁的onActivityResult()方法,并且在Activity的onRestart()生命周期回调之前!

在Fragment中调用**getActivity().startActivityForResult(intent, 200)**;

	04-25 10:13:22.414: E/####(1036): 执行了Activity A 的 onPause 
	04-25 10:13:22.844: E/####(1036): 执行了Activity A 的 onStop 
	04-25 10:13:25.134: E/####(1036): 执行了Activity A的回调!---->requestCode: 200resultCode :300
	04-25 10:13:25.134: E/####(1036): 执行了Activity A正确的回调:我是第4个条目
	04-25 10:13:25.134: E/####(1036): 随机数:59
	04-25 10:13:25.134: E/####(1036): 执行了Activity A 的 onRestart 
	04-25 10:13:25.134: E/####(1036): 执行了Activity A 的 onResume 
//-----------------------------------------------------------------------------------

在Fragment中调用**startActivityForResult(intent, 200)**;

	04-25 10:16:36.674: E/####(2235): 执行了Activity A 的 onResume 
	04-25 10:16:39.354: E/####(2235): 执行了Activity A 的 onPause 
	04-25 10:16:39.794: E/####(2235): 执行了Activity A 的 onStop 
	04-25 10:16:40.984: E/####(2235): 执行了Fragment A的回调!---->requestCode: 200resultCode :300
	04-25 10:16:40.984: E/####(2235): 执行了Fragment A正确的回调:我是第8个条目
	04-25 10:16:40.984: E/####(2235): 随机数:25
	04-25 10:16:40.984: E/####(2235): 执行了Activity A 的 onRestart 
	04-25 10:16:40.984: E/####(2235): 执行了Activity A 的 onResume 

在第一个Activity中:

	@Override
	protected void onActivityResult(int requestCode, int resultCode, Intent data) {
		super.onActivityResult(requestCode, resultCode, data);
		Log.e("####", "执行了Activity A的回调!"+"---->"+"requestCode: "+requestCode+"resultCode :"+resultCode);
		if (requestCode==200 && resultCode==300) {
			if (data!=null) {
				String result = data.getStringExtra("result");
				Log.e("####", "执行了Activity A正确的回调:"+result);
			}else {
				Log.e("####", "执行了Activity A正确的回调:"+"但是返回的数据为空!");
			}
		}
	}

在第一个Activity的Fragment中:

	@Override
	public void onActivityResult(int requestCode, int resultCode, Intent data) {
		super.onActivityResult(requestCode, resultCode, data);
		Log.e("####", "执行了Fragment A的回调!"+"---->"+"requestCode: "+requestCode+"resultCode :"+resultCode);
		if (requestCode==200 && resultCode==300) {
			if (data!=null) {
				String result = data.getStringExtra("result");
				Log.e("####", "执行了Fragment A正确的回调:"+result);
			}else {
				Log.e("####", "执行了Fragment A正确的回调:"+"但是返回的数据为空!");
			}
		}
	}

在Fragment中的Listview中点击题目启动展示Activity:

	@Override
		public View getView(int position, View convertView, ViewGroup parent) {
			ViewHolder holder;
			if (convertView==null) {
				holder=new ViewHolder();
				convertView=View.inflate(getActivity(), R.layout.item_fragment_listview_test1, null);
				holder.textView=(TextView) convertView.findViewById(R.id.textview);
				convertView.setTag(holder);
			}
			holder=(ViewHolder) convertView.getTag();
			holder.textView.setText("我是第"+position + "个条目");
			holder.textView.setOnClickListener(new OnClickListener() {
				
				@Override
				public void onClick(View v) {
					String string = ((TextView)v).getText().toString();
					gotoActvity(string);
				}
			});
			return convertView;
		}
		//.....

	private void gotoActvity(String text) {
		Intent intent=new Intent(getActivity(), Test1_1Activity.class);
		intent.putExtra("text", text);
		startActivityForResult(intent, 200);
		//getActivity().startActivityForResult(intent, 200);
		//谁调用,走谁的onActivityResult()方法,并且在Activity的onRestart()生命周期回调之前!
	}

