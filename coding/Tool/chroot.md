# Linux 命令：chroot

--<font color='red'>本质：</font>

> 1、隔离文件系统，类似于  Namespace
>
> 2、

好处：

> 基于 **隔离文件系统** --------> 自然：安全性：~~新根下将访问不到旧系统的根目录结构和文件，这样就增强了系统的安全性~~



## 例子

语法：



## 参考：

好文：

> https://blog.csdn.net/jiaoyangwm/article/details/130308127   【Linux 命令】chroot
>
> ----------------->  从最基本chroot来构建一个文件系统



其他：





# Linux 系统启动过程

https://blog.csdn.net/m0_65541699/article/details/127944130  Linux 系统启动过程



# linuxdeploy 

linuxdeploy 的原材料可以是iso，img等

比如：

```java
linuxdeploy -arch arm -v ubuntu -f ext4 -p /sdcard/ubuntu.img
```



参考：  https://www.python100.com/html/89007.html



# 源码：

https://github.com/meefik/linuxdeploy-cli

# 背后的背后 



# 抛开chroot