# 目录

# surfaceflinger的启动 之 .rc文件

参考：

> https://www.jianshu.com/p/dcc37f81c119

系统文件：

```java
 /system/bin/surfaceflinger
```

如何启动系统文件surfaceflinger：

> 1、配置
>
> ```java
>  /system/etc/init/surfaceflinger.rc
>  
>  service surfaceflinger /system/bin/surfaceflinger   // -----> 定义surfaceflinger服务
>      class core animation
>      user system                     //  ------> 用户
>      group graphics drmrpc readproc  // -----> 用户组
>      onrestart restart zygote        // -----> 重启条件
>      writepid /dev/stune/foreground/tasks   // ------>  surfaceflinger放到什么调频组
>      socket pdx/system/vr/display/client     stream 0666 system graphics u:object_r:pdx_display_client_endpoint_socket:s0
>      socket pdx/system/vr/display/manager    stream 0666 system graphics u:object_r:pdx_display_manager_endpoint_socket:s0
>      socket pdx/system/vr/display/vsync      stream 0666 system graphics u:object_r:pdx_display_vsync_endpoint_socket:s0
> ```
>
> 2、按照配置启动
>
> LoadBootScripts解析
>
> ParseConfigDir    解析路径，files收集目录下所有文件   ----> 不得不
>
> for (file : files)  遍历所有文件
>
> ParseConfigFile 解析文件
>
> ParseData 解析数据
>
> ServiceParser::ParseLineSection   解析行时，根据不同的关键词，选择不同的SectionParser。比如：service 选择  ServiceParser；import 选择 ImportParser
>
> ServiceParser::ParseGroup
>
> service_->proc_attr.gid = gid;   // 【】  最终赋值点

# ==进入surfaceflinger  main 之后==

参考：

> https://blog.csdn.net/qq_40587575/article/details/129657882     【安卓源码】SurfaceFlinger 启动及其与应用通信
>
> ----------> 源码注释

源码：

TODO:

设计美好： 统一的配置，剥离成文件