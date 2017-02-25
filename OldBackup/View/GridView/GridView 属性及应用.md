## GridView 属性及应用 ##

#### 1. paddingTop ####
	当设置了paddingtop的值后，滚动时内容没有穿过paddingtop的区域，
	解决办法是设置android:clipToPadding="false"

#### 2. paddingRight ####
	当设置paddingright属性时，滚动条显示在里边，要想滚动条显示在外边，
	设置android:scrollbarStyle=" o utsideOverlay "属性即可。

#### 3. item周围的空白 ####
	gridview的item两边显示多余的空白部分，设置 android:stretchMode="none"属性即可。
#### 4. item 的 onclick事件 ####
	gridview调用setonitemclicklistener时确保item的clickable属性为false.