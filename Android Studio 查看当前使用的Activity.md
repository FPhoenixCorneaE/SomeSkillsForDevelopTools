在 Android Studio 中，当使用真机或者模拟器调试应用时，应用运行到某一个页面。而开发人员不知道当前 UI 的类的时候，可以通过如下方法快速找到对应的activity.



### 第一种：通过如下命令：

#### 1.adb shell

#### 2.dumpsys activity | grep 'mFoc' 



### 第二种：通过如下命令：

#### 1.adb shell

#### 2.dumpsys activity top | grep "ACTIVITY" -A 0



### 第三种：通过关键字：ActivityManager 查找 logcat 打印日志



#### 第四种：Tools -> Layout Inspector

