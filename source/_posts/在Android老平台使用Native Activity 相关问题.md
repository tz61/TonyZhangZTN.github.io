# 在Android老平台使用Native Activity 相关问题

Problem1

```bash
2023-01-29 15:11:40.477 30209-30209/ztn.edu.native052119 E/AndroidRuntime: FATAL EXCEPTION: main
    Process: ztn.edu.native052119, PID: 30209
    java.lang.RuntimeException: Unable to start activity ComponentInfo{ztn.edu.native052119/android.app.NativeActivity}: java.lang.IllegalArgumentException: Unable to load native library: /data/app/ztn.edu.native052119-1/lib/arm/libmain.so
        at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2303)
        at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:2363)
        at android.app.ActivityThread.access$800(ActivityThread.java:147)
        at android.app.ActivityThread$H.handleMessage(ActivityThread.java
:1283)
...
```

Reference:[Loading 3rd party shared libraries from an Android native activity - Stack Overflow](https://stackoverflow.com/questions/8828559/loading-3rd-party-shared-libraries-from-an-android-native-activity)

Solution: 注册一个新的Activity ，并添加main action intent，在该onCreate方法中先调用System.loadLibrary("assimp"); 然后再启动NativeActivity

Example: 

```java
public class MyNativeActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        System.loadLibrary("assimp");
        System.loadLibrary("main");
        Log.d("test","test");
        super.onCreate(savedInstanceState);

        Intent intent = new Intent(MyNativeActivity.this, android.app.NativeActivity.class);
        MyNativeActivity.this.startActivity(intent);
    }
}
```

AndroidManifest.xml

```xml

<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="ztn.edu.native052119">
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MyNativeActivity"
            android:label="@string/app_name"
            android:configChanges="orientation|keyboardHidden">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <action android:name="android.intent.action.VIEW"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
        <activity android:name="android.app.NativeActivity"
            android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
            android:allowBackup="true"
            android:icon="@mipmap/ic_launcher"
            android:label="@string/app_name"
            android:roundIcon="@mipmap/ic_launcher_round"
            >
            <meta-data android:name="android.app.lib_name"
                android:value="main"/>

        </activity>
    </application>
</manifest>


```



Problem2:

Shared libraries cannot locate symbol "__register_atfork"

Reference:

[Shared libraries cannot locate symbol "__register_atfork" · Issue #964 · android/ndk · GitHub](https://github.com/android/ndk/issues/964)

Solution:

不要在老的api机子上用新的api版本的库


























