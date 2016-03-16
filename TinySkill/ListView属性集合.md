## Listview属性集合 ##
### 1. 基本属性: ###
	android:cacheColorHint="#00000000"  //设置拖动背景色为透明 
	android:dividerHeight="30px"         //listview item之间的高度
	android:divider="@drawable/ic_launcher"    //listview item之间的背景或者说是颜色
	android:fadingEdge="vertical"         //上边和下边有黑色的阴影      值为none的话就没有阴影
	android:scrollbars="horizontal|none"   //只有值为horizontal|vertical的时候，才会显示滚动条，并且会自动影藏和显示
	android:fastScrollEnabled="true"        //快速滚动效果，配置这个属性，在快速滚动的时候旁边会出现一个小方块的快速滚动效果，自动隐藏和显示，
### 2.容易混淆属性 ###
	 android:scrollbarStyle="outsideInset"  //四个值的含义如下
	 1>outsideInset :  该ScrollBar显示在视图(view)的边缘,增加了view的padding. 如果可能的话,该ScrollBar仅仅覆盖这个view的背景.
	 2>outsideOverlay :  该ScrollBar显示在视图(view)的边缘,不增加view的padding,该ScrollBar将被半透明覆盖
	 3>insideInset :该ScrollBar显示在padding区域里面,增加了控件的padding区域,该ScrollBar不会和视图的内容重叠.
	 4>insideOverlay : 该ScrollBar显示在内容区域里面,不会增加了控件的padding区域,该ScrollBar以半透明的样式覆盖在视图(view)的内容上.

### 3.详解: ###
	第一是stackFromBottom属性，这只该属性之后你做好的列表就会显示你列表的最下面，值为true和false
	android:stackFromBottom="true"      
	       
	第二是 transciptMode属性，需要用ListView或者其它显示大量Items的控件实时跟踪或者查看信息，并且希望最新的条目可以自动滚动到可视 范围内。通过设置的控件transcriptMode属性可以将Android平台的控件（支持ScrollBar）自动滑动到最底部。 android:transcriptMode="alwaysScroll"   
	
	第三cacheColorHint属性，很多人希望能够改变一下它的背景，使他能够符合整体的UI设计，改变背景背很简单只需要准备一张图片然后指定属性 android:background="@drawable/bg"，不过不要高兴地太早，当你这么做以后，发现背景是变了，但是当你拖动，或者点击list空白位置的时候发现ListItem都变成黑色的了，破坏了整体效果。
	如果你只是换背景的颜色的话，可以直接指定android:cacheColorHint为你所要的颜色，如果你是用图片做背景的话，那也只要将android:cacheColorHint指定为透明（#00000000）就可以了
	
	第四divider属性，该属性作用是每一项之间需要设置一个图片做为间隔，或是去掉item之间的分割线
	android:divider="@drawable/list_driver"  其中  @drawable/list_driver 是一个图片资源，如果不想显示分割线则只要设置为android:divider="@drawable/@null" 就可以了
	
	第五fadingEdge属性，上边和下边有黑色的阴影
	android:fadingEdge="none" 设置后没有阴影了~

	第六scrollbars属性，作用是隐藏listView的滚动条，
	android:scrollbars="none"与setVerticalScrollBarEnabled(true);的效果是一样的，不活动的时候隐藏，活动的时候也隐藏

	第七fadeScrollbars属性:

	android:fadeScrollbars="true"  配置ListView布局的时候，设置这个属性为true就可以实现滚动条的自动隐藏和显示。
	

	8.fastScrollEnabled属性：
	
	如果你使用XML布局，只需要在ListView节点中加入  android:fastScrollEnabled="true" 这个属性即可，而对于Java代码可以通过myListView.setFastScrollEnabled(true); 来控制启用，参数false为隐藏。
	
	还有一点就是当你的滚动内容较小，不到当前ListView的3个屏幕高度时则不会出现这个快速滚动滑块，同时该方法仍然是AbsListView的基础方法，可以在ListView或GridView等子类中使用快速滚动辅助。
	
	
	9.drawSelectorOnTop属性：
	
	android:drawSelectorOnTop="true" 点击某一条记录，颜色会显示在最上面，记录上的文字被遮住，所以点击文字不放，文字就看不到
	
	android:drawSelectorOnTop="false"点击某条记录不放，颜色会在记录的后面，成为背景色，但是记录内容的文字是可见的，属性默认值是false.


