<details>
<summary>Log Dump

``` bash
10-18 11:21:18.563  7371  7371 W System.err: java.lang.NullPointerException: Attempt to invoke virtual method 'int android.view.View.getVisibility()' on a null object reference
10-18 11:21:18.564  7371  7371 W System.err:    at android.view.ViewRootImpl.getHostVisibility(ViewRootImpl.java:1861)
10-18 11:21:18.564  7371  7371 W System.err:    at android.view.ViewRootImpl.handleAppVisibility(ViewRootImpl.java:1496)
10-18 11:21:18.564  7371  7371 W System.err:    at android.view.ViewRootImpl$ViewRootHandler.handleMessage(ViewRootImpl.java:4937)
10-18 11:21:18.564  7371  7371 W System.err:    at android.os.Handler.dispatchMessage(Handler.java:106)
10-18 11:21:18.564  7371  7371 W System.err:    at android.os.Looper.loop(Looper.java:216)
10-18 11:21:18.564  7371  7371 W System.err:    at android.app.ActivityThread.main(ActivityThread.java:7266)
10-18 11:21:18.564  7371  7371 W System.err:    at java.lang.reflect.Method.invoke(Native Method)
10-18 11:21:18.565  7371  7371 W System.err:    at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:494)
10-18 11:21:18.565  7371  7371 W System.err:    at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:975)
```
</details>
