### 第一步：配置 .gradle 文件夹目录（开启 Gradle 单独守护线程）。

在 windows 系统的 C:\Users\用户名\.gradle 目录下创建 gradle.properties 文件（有直接用），然后添加以下内容，
添加之后会在所以的项目中生效（有内容则并入），添加后全局生效

```groovy
org.gradle.daemon=true // 开启线程守护，第一次编译时开线程，之后就不会再开了 
org.gradle.jvmargs=-Xmx8g -XX:MaxPermSize=1g -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8 // 配置编译时的虚拟机大小 
org.gradle.parallel=true // 开启并行编译，相当于多条线程再走 
org.gradle.configureondemand=true //启用新的孵化模式
android.enableBuildCache=true  //开启缓存
```


如果在当前项目中的 gradle.properties 文件中添加以上内容，则只会在当前项目生效。

​		

### 第二步：修改 android studio 配置（根据实际开发情况，可忽略）。

1.打开设置选项卡，找到 Gradle 选项，选中 offline work，点击 apply。
2.找到 Compiler 选项，选中 compile independent modules in parallel 和 configure on demand。
command-line options 填写 --offline

​		

### 第三步：在具体开发module的build.gradle文件中添加。

```groovy
dexOptions {
    //使用增量模式构建，gradle高版本或已删除该属性 
    incremental true
    //最大堆内存 
    javaMaxHeapSize "8g"
    //是否支持大工程模式 
    jumboMode true
    //预编译 
    preDexLibraries true
    //
    dexInProcess true
    //最大进程数
    maxProcessCount 8
    //线程数 
    threadCount 8
}

//防止aapt工具可能会填充已压缩的PNG文件以加快编译速度
aaptOptions{
    cruncherEnabled = false
}
```

​		

### 第四步：在Project的build.gradle文件中的allprojects里边添加。

```groovy
gradle.taskGraph.whenReady {
    tasks.each { task ->
        if (task.name.contains("Test")) {
            task.enabled = false
        }
    }
}
```


​		
​		

### 第五步：尽量使用aar依赖代替Module依赖，把aar文件放在libs 目录下，在build.gradle 添加。

```groovy
allprojects {
    repositories {
        jcenter()
        flatDir {
            dirs 'libs'
        }
    }
}

dependencies {
    compile(name:'aar文件名', ext:'aar')
}
```

​		
​		

### 第六步：项目插件化改造，减少setting.gradle 里的 module 数量。

​		

### 最后，如果编译还是很慢的话，那就只能添加内存条和固态硬盘了！





