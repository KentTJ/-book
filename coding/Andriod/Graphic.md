# 目录



# 图形

[TOC]

# 系统（脱离安卓来看）

## 图形系统0层

![image-20230419002607069](Graphic.assets/image-20230419002607069.png)

图片： https://mp.weixin.qq.com/s/LVVR1IGrn_PopGUkefjKvA   深入理解Android图形系统,   [Linux阅码场](javascript:void(0);)



对于任何图形系统（~~抛开安卓不谈~~），**为什么需要这些？  不得不**：

>  **GUI需要**：
>
> 1、Render系统：
>
> ​                       作用： view界面数据 到Buffer上（~~实际上是绘制控件~~）： 即 <font color='red'>执行GUI的绘图指令集</font> 
>
> ​                        位置：Render线程
>
> 2、window系统 ： （1）窗口的管理器 ，对于安卓，位于WMS 中
>
> ​                                 （2）窗口的合成器 ---->  注：对于安卓是surfaceFlinger
>
> 3、DisPlay系统： 
>
> ​                       作用：把Buffer的位图显示出来
>
> ​                        位置：内核驱动

总之：

图形系统 = window系统 + Render系统 + DisPlay系统



## 图形系统1层

render系统:  1、是以 lib（.so）形式存在   2、运行时：在App GUI进程里





![64](Graphic.assets/64.png)



### **渲染系统**

目标：

> view数据转buffer数据

![65](Graphic.assets/65.png)



基于目标，渲染引擎有哪些？

> 2D引擎 Skia，3D引擎 OpenGL ES，RenderScript，OpenCV和Vulkan







### Window系统（图形视角）

目标：

> 区别各个窗口之间
>
> 协调各窗口之间的关系



![Image](Graphic.assets/640)



### 显示系统

目标：

将buffer数据，最终到显示屏上





# 显示驱动模型DRM架构介绍（一）

https://www.eet-china.com/mp/a178945.html          [**![img](Graphic.assets/avatar.php)** Linux阅码场](https://www.eet-china.com/mp/u3946005)

