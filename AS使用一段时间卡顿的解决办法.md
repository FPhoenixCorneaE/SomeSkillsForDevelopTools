Help选项 --> 选择Edit Custom VM Options...

如果没有此文件，新建一个
粘贴以下代码
也就是为Android Studio分配最小内存和最大内存

文件路径：C:\Users\Administrator\.AndroidStudio3.2\config\studio64.exe.vmoptions

-Xms2048m
-Xmx8196m
-XX:MaxPermSize=2048m
-XX:ReservedCodeCacheSize=2048m
-ea
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-Djna.nosys=true
-Djna.boot.library.path=

-Djna.debug_load=true
-Djna.debug_load.jna=true
-Djsse.enableSNIExtension=true
-XX:+UseCodeCacheFlushing
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-Didea.platform.prefix=AndroidStudio
-Didea.paths.selector=AndroidStudio

-da


然后选择File -->选择Invalidate Caches/Restart...
