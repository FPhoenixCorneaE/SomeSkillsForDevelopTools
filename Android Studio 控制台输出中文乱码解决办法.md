# Android Studio 控制台输出中文乱码

### 解决办法：

1. 点击工具栏上的放大镜或双击 Shift

2. 全局搜索 Edit Custom VM Options...

3. 在打开的 studio64.exe.vmoptions 文件，文件末尾加入：

   `-Dfile.encoding=UTF-8`

4. 重启 Android Studio