## 百度地图初始化错误-- No implementation found for int com.baidu.platform.comjni.map.commonmemcache.JNICommonMemCache.Create() ##

1.**`No implementation found for int com.baidu.platform.comjni.map.commonmemcache.JNICommonMemCache.Create()`**原因:

**你的其他的 `库` 是否存在 `armeabi`和`armeabi-v7a`以外的`so`文件夹,有的话删掉.(前提是你的库里只含有`armeabi`和`armeabi-v7a`文件夹)**