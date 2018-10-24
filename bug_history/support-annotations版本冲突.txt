1、问题：
新创建项目的时候突然出现

```
Error:Execution failed for task ':app:preDebugAndroidTestBuild'.
> Conflict with dependency 'com.android.support:support-annotations' in project ':app'. 
Resolved versions for app (26.1.0) and test app (27.1.1) differ. See https://d.android.com/r/tools/test-apk-dependency-conflicts.html for details.
```

这个问题，但是这个问题不影响程序运行，只是每次编译都会出现。

2、解决过程
根据com.android.support:support-annotations 提示，查看External Libraies下，发现有两个版本 一个是26.1.0一个是27.1.1 
猜测是与程序运行SDK版本有关
当前版本

```

compileSdkVersion 26
    defaultConfig {
        applicationId "com.example.administrator.myapplication"
        minSdkVersion 15
        targetSdkVersion 26
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
```

当前compileSdkVersion 为26 所以每次运行时编译的是 26.1.0的版本，但是系统找到的一直是最新的27.1.1版本 ，因此出现以上异常提示。
（猜测系统会找最新的的版本，因为在大白项目中未出现异常，查看External Libraies发现最新的版本为26.10，compileSdkVersion 为26。）

3、解决方案1：
   将compileSdkversion和targetVersion统一升级到27+ 再次编译解决。
   解决方案2：
   在app build.gradle的dependencies 中添加

```
   androidTestCompile('com.android.support:support-annotations:26.1.0') {
        force = true
    }
```


