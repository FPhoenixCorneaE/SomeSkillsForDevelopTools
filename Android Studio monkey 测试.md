### 如何使用 Android monkey 进行自动化测试？

上线一个app，如果崩溃率高，会造成辛辛苦苦拉来的用户流失。为了减少这种情况，用Android monkey这个自动化测试工具来进行压力测试。Android monkey就是模拟用户触摸屏幕、滑动Trackball、 按键等随机操作，可以很方便进行上百万次的测试。

### 工具

* Android Studio
* bugly
* 安卓手机并打开USB调试

### 步骤

#### 1.使用 Android Studio 编写好项目代码，首先开发人员要遵守严格的编程规范，写出高健壮的代码，进行单元测试等，反复检查写的代码，严格把关减少出现bug，特别是崩溃。

#### 2.打开命令行，连接手机并安装好要测试的 app，输入 adb shell，看手机是否连接。

#### 3.如果不清楚如何使用Monkey的参数，可以在命令行输入：

##### adb shell monkey -help

#### 4.在 Terminal 命令行输入以下命令，就可以运行 Monkey,可以看到在手机运行指定的 app,并进行各种乱点乱按：

##### adb shell monkey -p com.xxx.xxx(包名) 100000(次数) -v 1000 throttle 500 —ignore-crashes —ignore-timeouts —ignore-security-exceptions —monitor-native-crashes > C:\Users\xxx\Desktop\crash.txt

#### 5.Monkey有各种参数满足各种高级需求，如-v指定日志详细程度，可以指定触摸事件、轨迹球事件、导航事件百分比。

#### 6.集成bugly等质量跟踪平台，在后台看Monkey测出来的崩溃，及时更正。

#### 