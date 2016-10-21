## TextView 使用(慢慢更新)

### 下划线(划线)
1.textView设置下划线
textView.getPaint().setFlags(Paint. UNDERLINE_TEXT_FLAG ); //下划线
textView.getPaint().setAntiAlias(true);//设置下划线并抗锯齿(加清晰)

2.textView设置中划线
textview.getPaint().setFlags(Paint. STRIKE_THRU_TEXT_FLAG); //中划线
setFlags(Paint. STRIKE_THRU_TEXT_FLAG|Paint.ANTI_ALIAS_FLAG);  // 设置中划线并加清晰

3.textView取消中划线或者下划线
textView.getPaint().setFlags(0);  // 取消设置的的划线
Android如何设置TextView的行间距、行高。

### 行间距
Android系统中TextView默认行间距比较窄，不美观。
我们可以设置每行的行间距，可以通过属性android:lineSpacingExtra或android:lineSpacingMultiplier来做。
在你要设置的TextView中加入如下代码：

1、android:lineSpacingExtra 
设置行间距，如”8dp”。

2、android:lineSpacingMultiplier 
设置行间距的倍数，如”1.5″。