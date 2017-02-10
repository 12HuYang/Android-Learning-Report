##Android 和 JS 交互时调用不成功的问题

###1.开启JavaScript
	webView.getSettings().setJavaScriptEnabled(true);设置WebView支持JavaScript
###2.Android与Js绑定操作
	webView.addJavascriptInterface(new AnalysisJS(), ANALYSIS_JS);
	绑定（这样解释）一个java对象到webview，其实就是将一个java对象和网页JS联系起来
###3.增加注解
	增加@JavascriptInterface注解，需要导入import android.webkit.JavascriptInterface;
###4.查看清单文件TargetSDK:16及以下
	若上面3个都已添加，还是无法调用。
	请检查清单文件(AndroidManifest.xml)中android:targetSdkVersion="16" ，这里targetSdkVersion改成16（17以下应该都可以，未测试）。
****
####原因猜想：
当你的targetSdkVersion为17以上时，addJavascriptInterface会提示错误：“ None of the methods in the added interface have been annotated with @android.webkit.JavascriptInterface; they will not be visible in API 17 ”。大概意思就是说在注解@JavascriptInterface中的方法在API 17会不可见。以此来推断，你在电脑上用17以上的sdk编译你的java代码为.class字节码文件时，是会出问题的。

####我的原因:(2016/3/12 12:03:01)
提示:"捕获到没有定义的方法."; 