###类找不到异常:
### Caused by: java.lang.ClassNotFoundException

	LogCat的更多输出日志:

	09-05 09:18:06.127: E/AndroidRuntime(981): FATAL EXCEPTION: main
	09-05 09:18:06.127: E/AndroidRuntime(981): java.lang.RuntimeException: Unable to get provider com.example.de.vogella.android.todos.contentprovider.MyTodoContentProvider: java.lang.ClassNotFoundException: Didn't find class "com.example.de.vogella.android.todos.contentprovider.MyTodoContentProvider" on path: DexPathList[[zip file "/data/app/com.example.de.vogella.android.todos-1.apk"],nativeLibraryDirectories=[/data/app-lib/com.example.de.vogella.android.todos-1, /system/lib]]
	09-05 09:18:06.127: E/AndroidRuntime(981):  at android.app.ActivityThread.installProvider(ActivityThread.java:4882)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at android.app.ActivityThread.installContentProviders(ActivityThread.java:4485)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at android.app.ActivityThread.handleBindApplication(ActivityThread.java:4425)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at android.app.ActivityThread.access$1300(ActivityThread.java:141)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1316)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at android.os.Handler.dispatchMessage(Handler.java:99)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at android.os.Looper.loop(Looper.java:137)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at android.app.ActivityThread.main(ActivityThread.java:5103)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at java.lang.reflect.Method.invokeNative(Native Method)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at java.lang.reflect.Method.invoke(Method.java:525)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:737)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:553)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at dalvik.system.NativeStart.main(Native Method)
	09-05 09:18:06.127: E/AndroidRuntime(981): Caused by: java.lang.ClassNotFoundException: Didn't find class "com.example.de.vogella.android.todos.contentprovider.MyTodoContentProvider" on path: DexPathList[[zip file "/data/app/com.example.de.vogella.android.todos-1.apk"],nativeLibraryDirectories=[/data/app-lib/com.example.de.vogella.android.todos-1, /system/lib]]
	09-05 09:18:06.127: E/AndroidRuntime(981):  at dalvik.system.BaseDexClassLoader.findClass(BaseDexClassLoader.java:53)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at java.lang.ClassLoader.loadClass(ClassLoader.java:501)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at java.lang.ClassLoader.loadClass(ClassLoader.java:461)
	09-05 09:18:06.127: E/AndroidRuntime(981):  at android.app.ActivityThread.installProvider(ActivityThread.java:4867)
	09-05 09:18:06.127: E/AndroidRuntime(981):  ... 12 more
	09-05 09:18:06.156: E/ActivityThread(950): Failed to find provider info for de.vogella.android.todos.contentprovider

####解决方法:
	

	I had this problem once. The code was working previously and suddenly it stopped working (crash at app startup) when I synced and built an older version of the code.
	
	The fix was to just close and restart Eclipse and clean the project and clean all the dependent library projects. Then it started working properly again.
	
	It's some sort of build problem in Eclipse, when refreshing the project files.

	(这是重点)
	Update: In particular, if you've accidentally modified the ".classpath" file (to revert to an older version),
	Eclipse/Android SDK can get confused and not build the project properly.
	When you restart Eclipse and clean the project, Eclipse will re-modify the ".classpath" file, and build properly.
	
	edited Dec 6 '13 at 13:59
	answered Dec 6 '13 at 10:35 