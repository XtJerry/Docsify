``` java
// Application 类
public class App extends MultiDexApplication {
	@Override
    public void onCreate() {
        super.onCreate();
		
		String curProcessName = getCurProcessName(this);
        if (!curProcessName.equals(getPackageName())) {
            return;
        }
	}
	
	public String getCurProcessName(Context cxt) {
        int pid = android.os.Process.myPid();
        ActivityManager am = (ActivityManager) cxt.getSystemService(Context.ACTIVITY_SERVICE);
        List<ActivityManager.RunningAppProcessInfo> runningApps = am.getRunningAppProcesses();
        if (runningApps == null) {
            return null;
        }
        for (ActivityManager.RunningAppProcessInfo procInfo : runningApps) {
            if (procInfo.pid == pid) {
                return procInfo.processName;
            }
        }
        return null;
    }
	...
	
}
```
