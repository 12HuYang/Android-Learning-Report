## Android 官方文档泛读 ##

> 2016/6/12 16:28:04 

#### Android 6.0 的主要变化 ####

1. **Runtime Permissions**:新的权限机制,官方的建议是:在您的应用程序适用于Android 6.0（API级别23）以上时，一定要检查在运行时和请求的权限。要确定您的应用程序被授予的权限，调用新的checkSelfPermission（） 方法。要请求权限，调用新的 requestPermissions（） 方法。即使您的应用程序不是指定的Android 6.0（API等级23），你应该在新的权限模式下测试应用程序。


2. **Doze and App Standby** :新的待机机制.

3. **Apache的HTTP客户端删除**:使用HttpURLConnection类类代替。此API是更有效的，因为它减少了通过透明压缩和响应缓存网络的使用，并减少功耗。如果你一定要用,Google提供了Android studio的方法:在`build.gradle`添加:

	android {    useLibrary 'org.apache.http.legacy'}
4. **访问硬件标识符**:为用户提供更高的数据保护，在此版本中开始，Android的删除程序访问设备的本地硬件标识符使用的Wi-Fi和蓝牙API的应用程序。所述 WifiInfo.getMacAddress（）和 BluetoothAdapter.getAddress（）方法现在返回恒定值02：00：00：00：00：00。要通过蓝牙和Wi-Fi扫描访问外部附近设备的硬件标识码，您的应用程序现在必须有ACCESS_FINE_LOCATION或 ACCESS_COARSE_LOCATION权限：

5. 