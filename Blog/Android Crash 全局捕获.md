# Android Crash 全局捕获
> 首先应该明白的一点是,Android在崩溃后会重新启动崩溃时的那个Activity,如果你的Activity在初始化的时候就直接崩溃,那么你将连续得到 Crash 崩溃日志.这个说出来可能没什么,可怜的我在看到崩溃日志时活脱脱的以为 `uncaughtException(Thread thread, Throwable ex)` 方法被调用了两次.知道原因后就很好解决,在崩溃时把 `Activity` 栈清空就可以.

## 1. UncaughtExceptionHandler
**这是一个接口,只有一个方法 `uncaughtException(Thread t, Throwable e)` 而且注释已经说明,这个方法是`虚拟机(JVM)`去调用.**所以我们编写一个类实现此接口就可以了.
```
    public interface UncaughtExceptionHandler {
        /**
         * Method invoked when the given thread terminates due to the
         * given uncaught exception.
         * <p>Any exception thrown by this method will be ignored by the
         * Java Virtual Machine.
         * @param t the thread
         * @param e the exception
         */
        void uncaughtException(Thread t, Throwable e);
    }
```

## 2. 异常时的常规处理
> **1. 显示崩溃前的`Toast`**
> **2. 保存崩溃日志信息到sd卡**
> **3. 重启APP**

实际项目中,我们需要做好这三件事.
```
    /**
     * 异常发生时，系统回调的函数，我们在这里处理一些操作
     */
    @Override
    public void uncaughtException(Thread thread, Throwable ex) {
        showCrashToast();//展示崩溃前的Toast
        saveCrashReport2SD(mContext, ex);//保存崩溃信息到sdcard
        restartApp();//重新启动APP
    }
```

## 3. 完整的代码
需要注意的地方写在注释里了.
```
public class CrashHandler implements UncaughtExceptionHandler {
    private static final String TAG = "CrashHandler";
    private Context mContext;
    private static CrashHandler mInstance = new CrashHandler();
    private CrashHandler() {
    }

    /**
     * 单例模式，保证只有一个CrashHandler实例存在
     *
     * @return
     */
    public static CrashHandler getInstance() {
        return mInstance;
    }

    /**
     * 异常发生时，系统回调的函数，我们在这里处理一些操作
     */
    @Override
    public void uncaughtException(Thread thread, Throwable ex) {
        showCrashToast();
        saveCrashReport2SD(mContext, ex);
        restartApp();
    }

    /**
     * 为我们的应用程序设置自定义Crash处理
     */
    public void initCrashHandler(Context context) {
        mContext = context;
        Thread.setDefaultUncaughtExceptionHandler(this);
    }

    private void restartApp() {
        Intent intent = new Intent(App.getInstance().getApplicationContext(),
                InitAdvsMultiActivity.class);
        intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP | Intent.FLAG_ACTIVITY_CLEAR_TASK);
        PendingIntent restartIntent = PendingIntent.getActivity(App.getInstance()
                .getApplicationContext(), 0, intent, PendingIntent.FLAG_ONE_SHOT);
        //重启应用
        AlarmManager mgr = (AlarmManager) App.getInstance().getSystemService(Context.ALARM_SERVICE);
        mgr.set(AlarmManager.RTC, System.currentTimeMillis(), restartIntent);

        //清空Activity栈,防止系统自动重启至崩溃页面,导致崩溃再次出现.
        ActivityLifeManager.getInstance().finishAllActivity();
        //退出程序
        android.os.Process.killProcess(android.os.Process.myPid());
        System.exit(0);
        System.gc();
    }

    private void showCrashToast() {
        new Thread() {
            @Override
            public void run() {
                Looper.prepare();
                Toast.makeText(App.getInstance().getApplicationContext(), "很抱歉,程序出现异常,即将重启",
                        Toast.LENGTH_LONG).show();
                Looper.loop();
            }
        }.start();
        try {
            Thread.sleep(1000);//Toast展示的时间
        } catch (InterruptedException e) {
        }
    }

    /**
     * 获取一些简单的信息,软件版本，手机版本，型号等信息存放在LinkedHashMap中
     *
     * @param context
     * @return
     */
    private HashMap<String, String> obtainSimpleInfo(Context context) {
        LinkedHashMap<String, String> map = new LinkedHashMap<String, String>();
        PackageManager mPackageManager = context.getPackageManager();
        PackageInfo mPackageInfo = null;
        try {
            mPackageInfo = mPackageManager.getPackageInfo(context.getPackageName(),
                    PackageManager.GET_ACTIVITIES);
        } catch (NameNotFoundException e) {
            e.printStackTrace();
        }

        map.put("APP", "AppName");
        map.put("用户名", SharedPreferencesUtils.getInstance().getUserName());
        map.put("品牌", "" + Build.BRAND);
        map.put("型号", "" + Build.MODEL);
        map.put("SDK版本", "" + Build.VERSION.SDK_INT);
        map.put("versionName", mPackageInfo.versionName);
        map.put("versionCode", "" + mPackageInfo.versionCode);
        map.put("crash时间", parserTime(System.currentTimeMillis()));

        return map;
    }


    /**
     * 获取系统未捕捉的错误信息
     *
     * @param throwable
     * @return
     */
    private String obtainExceptionInfo(Throwable throwable) {
        StringWriter mStringWriter = new StringWriter();
        PrintWriter mPrintWriter = new PrintWriter(mStringWriter);
        throwable.printStackTrace(mPrintWriter);
        mPrintWriter.close();

        Log.e(TAG, mStringWriter.toString());
        return mStringWriter.toString();
    }

    /**
     * 保存获取的 软件信息，设备信息和出错信息保存在SDcard中
     *
     * @param context
     * @param ex
     * @return
     */
    private String saveCrashReport2SD(Context context, Throwable ex) {
        String fileName = null;
        StringBuilder sb = new StringBuilder();
        for (Map.Entry<String, String> entry : obtainSimpleInfo(context).entrySet()) {
            String key = entry.getKey();
            String value = entry.getValue();
            sb.append(key).append(" = ").append(value).append("\n");
        }
        sb.append(obtainExceptionInfo(ex));
        if (Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED)) {
            File dir = new File(CrashConstant.CRASH_DIR);
            if (!dir.exists()) {
                dir.mkdirs();
            }
            try {
                fileName = dir.toString() + File.separator + parserTime(System.currentTimeMillis()) +".txt";
                FileOutputStream fos = new FileOutputStream(fileName);
                fos.write(sb.toString().getBytes());
                fos.flush();
                fos.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        return fileName;
    }

    /**
     * 将毫秒数转换成yyyy-MM-dd-HH-mm-ss的格式，并在后缀加入随机数
     *
     * @param milliseconds
     * @return
     */
    private String parserTime(long milliseconds) {
        System.setProperty("user.timezone", "Asia/Shanghai");
        TimeZone tz = TimeZone.getTimeZone("Asia/Shanghai");
        TimeZone.setDefault(tz);
        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd-HH-mm-ss");
        return format.format(new Date(milliseconds));
    }
}
```