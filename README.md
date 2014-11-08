MultiDexTest
============

This app was written to reproduce a bug in Android build tools.


Reproduce environment
=========

1. Gradle plugin: com.android.tools.build:gradle:0.14.0
2. Compile SDK: android-21
3. SDK Build tools: 21.1.0
4. SDK tools: 23.0.5
5. SDK platform-tools: 21
6. Ubuntu & Android Studio 0.9.1

Bug description
=========

In latest SDK build tools, we can use the dx parameters '--multi-dex' & '--main-dex-list' & '--minimal-main-dex' to generate and control multiple dex files in APK. If these parameters were used, the expected dex files will be generated, it's great. But in the final APK, the dex files' names were changed.

From the following operation logs, you can see that the "classes*.dex" names in the final APK were exchanged!

```
[PWD: ~/work/tyc/ycdev/demos/MultiDexTest/app]  (master)
$ ../gradlew clean aDebug
:app:clean
<...ignored...>
:app:assembleDebug

BUILD SUCCESSFUL

Total time: 38.889 secs
[PWD: ~/work/tyc/ycdev/demos/MultiDexTest/app]  (master)
$ ll build/intermediates/dex/debug/
×ÜÓÃÁ¿ 4280
drwxrwxr-x 2 yongce yongce    4096 11ÔÂ  6 00:16 ./
drwxrwxr-x 3 yongce yongce    4096 11ÔÂ  6 00:16 ../
-rw-rw-r-- 1 yongce yongce  874148 11ÔÂ  6 00:16 classes2.dex
-rw-rw-r-- 1 yongce yongce 3494932 11ÔÂ  6 00:16 classes.dex
[PWD: ~/work/tyc/ycdev/demos/MultiDexTest/app]  (master)
$ unzip -l build/outputs/apk/app-debug.apk | grep ".dex"
   874148  2014-11-06 00:16   classes.dex
  3494932  2014-11-06 00:16   classes2.dex
```

AOSP bug track:
https://code.google.com/p/android/issues/detail?id=78761
