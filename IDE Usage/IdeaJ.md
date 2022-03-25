### 问题

#### 1 MacOs下 IdealJ 突然无法打开

##### 描述

在一次安装完plugin后，ideal提示需要重启软件，重启软件后Ideal无法打开。

这里所说的启动方式是通过 启动 或 应用栏 中的icon直接启动，现象是会一致卡在启动界面。

##### 解决

1，先找到终端触发的位置：

finder->右边栏->右键IntelliJ IDEA CE -> 显示包内容

![image](https://user-images.githubusercontent.com/13618018/160098500-c2cdf318-8c9f-4084-8038-b29fb2d88ad1.png)

2，手动执行IDE

./Applications/IntelliJ IDEA CE.app/Contents/MacOS/idea

这个操作会进入调试状态，实时打印启动日志。

3，定位具体问题

接下来我们去找具体的异常，日志较多，我们需要找到具体的异常，查找关键词 “ERROR”，比如我们发现有这种日志：

```
2017-07-11 17:22:48,947 [  22053]   INFO - ellij.project.impl.ProjectImpl - 23 project components initialized in 74 ms   
2017-07-11 17:22:48,947 [  22053]   INFO - le.impl.ModuleManagerComponent - 0 module(s) loaded in 0 ms   
2017-07-11 17:23:07,331 [  40437]   INFO - roject.impl.ProjectManagerImpl - com.intellij.diagnostic.PluginException: com.seventh7.mybatis.setting.MybatisSetting cannot be cast to com.seventh7.mybatis.setting.MybatisSetting [Plugin: com.seventh7.plugin.mybatis]   
com.intellij.ide.plugins.PluginManager$StartupAbortedException: com.intellij.diagnostic.PluginException: com.seventh7.mybatis.setting.MybatisSetting cannot be cast to com.seventh7.mybatis.setting.MybatisSetting [Plugin: com.seventh7.plugin.mybatis]  
    at com.intellij.ide.plugins.PluginManager.handleComponentError(PluginManager.java:249)  
    at com.intellij.openapi.components.impl.PlatformComponentManagerImpl.handleInitComponentError(PlatformComponentManagerImpl.java:43)  
    at com.intellij.openapi.components.impl.ComponentManagerImpl$ComponentConfigComponentAdapter.getComponentInstance(ComponentManagerImpl.java:519)  
    at com.intellij.openapi.components.impl.ComponentManagerImpl.createComponents(ComponentManagerImpl.java:125)  
    at com.intellij.openapi.components.impl.ComponentManagerImpl.init(ComponentManagerImpl.java:109)  
    at com.intellij.openapi.components.impl.ComponentManagerImpl.init(ComponentManagerImpl.java:96)  
    at com.intellij.openapi.project.impl.ProjectImpl.init(ProjectImpl.java:287)  
    at com.intellij.openapi.project.impl.ProjectManagerImpl.a(ProjectManagerImpl.java:222)  
    at com.intellij.openapi.project.impl.ProjectManagerImpl.d(ProjectManagerImpl.java:459)  
    at com.intellij.openapi.project.impl.ProjectManagerImpl.access$100(ProjectManagerImpl.java:60)  
    at com.intellij.openapi.project.impl.ProjectManagerImpl$2.compute(ProjectManagerImpl.java:406)  
    at com.intellij.openapi.project.impl.ProjectManagerImpl$2.compute(ProjectManagerImpl.java:403)  
    at com.intellij.openapi.progress.Task$WithResult.run(Task.java:307)  
    at com.intellij.openapi.progress.impl.CoreProgressManager$TaskRunnable.run(CoreProgressManager.java:710)  
    at com.intellij.openapi.progress.impl.CoreProgressManager$11.run(CoreProgressManager.java:423)  
    at com.intellij.openapi.progress.impl.CoreProgressManager$3.run(CoreProgressManager.java:179)  
    at com.intellij.openapi.progress.impl.CoreProgressManager.a(CoreProgressManager.java:568)  
    at com.intellij.openapi.progress.impl.CoreProgressManager.executeProcessUnderProgress(CoreProgressManager.java:519)  
    at com.intellij.openapi.progress.impl.ProgressManagerImpl.executeProcessUnderProgress(ProgressManagerImpl.java:54)  
    at com.intellij.openapi.progress.impl.CoreProgressManager.runProcess(CoreProgressManager.java:164)  
    at com.intellij.openapi.application.impl.ApplicationImpl.a(ApplicationImpl.java:572)  
    at com.intellij.openapi.application.impl.ApplicationImpl$2.run(ApplicationImpl.java:309)  
    at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)  
    at java.util.concurrent.FutureTask.run(FutureTask.java:266)  
    at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)  
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)  
    at java.lang.Thread.run(Thread.java:745)  
Caused by: com.intellij.diagnostic.PluginException: com.seventh7.mybatis.setting.MybatisSetting cannot be cast to com.seventh7.mybatis.setting.MybatisSetting [Plugin: com.seventh7.plugin.mybatis]  
    ... 27 more  
Caused by: java.lang.ClassCastException: com.seventh7.mybatis.setting.MybatisSetting cannot be cast to com.seventh7.mybatis.setting.MybatisSetting  
    at com.seventh7.mybatis.setting.MybatisSetting.getInstance(MybatisSetting.java:56)  
    at com.seventh7.mybatis.ref.CmProject.initComponent(CmProject.java:59)  
    at com.intellij.openapi.components.impl.ComponentManagerImpl$ComponentConfigComponentAdapter.getComponentInstance(ComponentManagerImpl.java:501)  
    ... 24 more  
```

具体只需关注 “Plugin: com.seventh7.plugin.mybatis” 这块。我们需要做的就是删掉它。

不用心疼，IDE恢复后我们还能正常再次下载。

##### 操作

我们要找到这个plugin的位置，然后删掉。

在finder中，快捷键 shift+command+g

找到这个文件夹：~/Library/Application Support/JetBrains/IdeaIC2021.3/plugins/（MacOs的 IDEA路径基本不会变化）

<img width="579" alt="image" src="https://user-images.githubusercontent.com/13618018/160101757-b1f2a943-e2f1-4d92-bda5-320cffba29b1.png">

找到对应的plugin文件夹， 然后整个删掉

再次尝试启动，OK！


