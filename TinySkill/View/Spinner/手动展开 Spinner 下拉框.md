## 手动展开 Spinner 下拉框 ##


	//代码片段
	//....
	mTraingleDown.setOnClickListener(new OnClickListener() {
			
			@Override
			public void onClick(View v) {
				mSpinner.performClick();
			}
		});
	//....

调用spinner的**performClick( )**方法即可.