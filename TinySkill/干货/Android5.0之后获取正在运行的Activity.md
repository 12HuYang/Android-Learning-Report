## Android5.0之后获取正在运行的Activity ##

	private String getProcess() throws Exception {
	    if (Build.VERSION.SDK_INT >= 21) {
	        return getProcessNew();
	    } else {
	        return getProcessOld();
	    }
	}

	//API 21 and above
	private String getProcessNew() throws Exception {
	    String topPackageName = null;
	    UsageStatsManager usage = (UsageStatsManager) context.getSystemService("usagestats");
	    long time = System.currentTimeMillis();
	    List<UsageStats> stats = usage.queryUsageStats(UsageStatsManager.INTERVAL_DAILY, time - ONE_SECOND * 10, time);
	    if (stats != null) {
	        SortedMap<Long, UsageStats> runningTask = new TreeMap<Long,UsageStats>();
	        for (UsageStats usageStats : stats) {
	            runningTask.put(usageStats.getLastTimeUsed(), usageStats);
	        }
	        if (runningTask.isEmpty()) {
	            return null;
	        }
	        topPackageName =  runningTask.get(runningTask.lastKey()).getPackageName();
	    }
	    return topPackageName;
	}

	//API below 21
	@SuppressWarnings("deprecation")
	private String getProcessOld() throws Exception {
	    String topPackageName = null;
	    ActivityManager activity = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
	    List<RunningTaskInfo> runningTask = activity.getRunningTasks(1);
	    if (runningTask != null) {
	        RunningTaskInfo taskTop = runningTask.get(0);
	        ComponentName componentTop = taskTop.topActivity;
	        topPackageName = componentTop.getPackageName();
	    }
	    return topPackageName;
	}

	//required permissions
	<uses-permission android:name="android.permission.PACKAGE_USAGE_STATS"/>
	<uses-permission android:name="android.permission.GET_TASKS"/>