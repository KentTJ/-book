# Kwin

[KDE如何处理窗口管理？-腾讯云开发者社区 (tencent.com)](https://cloud.tencent.com/developer/techpedia/2002/14620)   ------->  没啥内容

https://wenku.baidu.com/view/540df51eac1ffc4ffe4733687e21af45b307fefa.html?_wkts_=1699511709596&bdQuery=kwin+架构

https://www.linuxprobe.com/kwin-kde.html      [KWin即将内置高级窗口分屏布局](https://www.linuxprobe.com/kwin-kde.html)

https://blog.csdn.net/a8039974/article/details/122867167   wayland详解   -----> **好文，** 虽然是Weston，但是应该与Kwin一样，都是实现了wayland协议

## kwin的架构和原理

是什么？

> kde桌面环境的，图形架构

Kwin主要由以下几个部分组成:

> 1.渲染引擎:Kwin使用OpenGL作为其渲染引擎，它负责将窗口和桌面渲染为图像。 ---->  说法有误，render在应用内。合成在 KWin
>
> KWin 是一个 X Window System 的窗口管理器和一个 Wayland 合成器
>
> 2.事件处理: Kwin通过Qt的事件系统来接收和处理用户输入。
>
> 3.窗口管理: Kwin负责管理窗口的创建、销毁和移动
>
> 4.特效和过渡:Kwin提供了许多特效和过渡效果，例如窗口淡入淡出、窗口滑动等

Kwin的原理是，当用户打开一个窗口时，Kwin会创建一个新的窗口对象，并为其分配一个唯一的标识符。然后，Kwin

会根据用户的操作和系统状态来管理窗口的显示和隐藏。当用户点击窗口时，Kwin会将其激活，并将其移动到屏幕中央。

此外，Kwin还提供了许多特效和过渡效果，使得用户能够更轻松地使用计算机

```
 SwitchEvent: 切换事件 (窗门切换)
 SwitchEvent 类通常包含以下属性:
 。SwitchEvent::Type: 表示切换事件的类型，可以是切换到上一个窗口、下一个窗口或特定窗口的事件
 。SwitchEvent::Modifiers: 表示切换事件时使用的修饰键，例如 Alt、Shift 等
 。SwitchEvent::Direction: 表示切换的方向，可以是向前切换或向后切换
 。SwitchEvent::Window: 表示切换事件所涉及的窗口对象
```

## kwin进程展开

```
 进�� USER      PR  NI    VIRT    RES    SHR � %CPU %MEM     TIME+ COMMAND
 15941 chen  20   0 5494012 220668 154216 S  2.2  2.8   4:08.60 QXcbEventReader  // ------->   事件
 15940 chen  20   0 5494012 220668 154216 S  1.9  2.8   4:23.12 kwin_x11    // ------->主线程，合成也在这里？
 16156 chen  20   0 5494012 220668 154216 S  0.3  2.8   1:50.21 llvmpipe-0
 16163 chen  20   0 5494012 220668 154216 S  0.3  2.8   1:05.68 llvmpipe-7
 16166 chen  20   0 5494012 220668 154216 S  0.3  2.8   1:06.32 llvmpipe-10
 16169 chen  20   0 5494012 220668 154216 S  0.3  2.8   1:34.99 llvmpipe-13
 16009 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:01.50 QDBusConnection
 16154 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.06 QQmlThread
 16157 chen  20   0 5494012 220668 154216 S  0.0  2.8   1:20.53 llvmpipe-1
 16158 chen  20   0 5494012 220668 154216 S  0.0  2.8   1:18.47 llvmpipe-2
 16159 chen  20   0 5494012 220668 154216 S  0.0  2.8   1:16.29 llvmpipe-3
 16160 chen  20   0 5494012 220668 154216 S  0.0  2.8   1:13.13 llvmpipe-4
 16161 chen  20   0 5494012 220668 154216 S  0.0  2.8   1:11.32 llvmpipe-5
 16162 chen  20   0 5494012 220668 154216 S  0.0  2.8   1:08.64 llvmpipe-6
 16164 chen  20   0 5494012 220668 154216 S  0.0  2.8   1:06.42 llvmpipe-8
 16165 chen  20   0 5494012 220668 154216 S  0.0  2.8   1:04.73 llvmpipe-9
 16167 chen  20   0 5494012 220668 154216 S  0.0  2.8   1:09.25 llvmpipe-11
 16168 chen  20   0 5494012 220668 154216 S  0.0  2.8   1:14.88 llvmpipe-12
 16170 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16171 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16172 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16173 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16174 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16175 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16176 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16177 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16178 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16179 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16180 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16181 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16182 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16183 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
 16187 chen  20   0 5494012 220668 154216 S  0.0  2.8   0:00.00 kwin_x11
```

注意：

> kwin相当于安卓的框架层，wms与合成器
>
> KDE，是launcher，桌面环境

## wayland详解

https://blog.csdn.net/a8039974/article/details/122867167   TODO

## kwin源码在线

https://invent.kde.org/plasma/kwin/-/blob/master/src/inputmethod.cpp

# kwin源码阅读

## 窗口管理之焦点窗口：

结构：**stacking_order  窗口栈结构**

```
 // layers.cpp
 stacking_order:
        增删改：addWaylandWindow、updateStackingOrder、removeFromStack、removeWindow
        查：findWindowToActivateOnDesktop、findToplevel
```

使用：

> ```
>  //input.cpp
>  const QList<Window *> &stacking = Workspace::self()->stackingOrder();
>  // ------->  维护了一个栈，大概率焦点窗口，就是
> ```
>
> 一句话核心：
>
> ```
>  window = input()->findToplevel(position());   // Top窗口，大概率认为是focus的
> ```
>
> 使用层：
>
> ```
>  InputDeviceHandler::setHover(Window *window)  ---->
>     // 传给Input模块
>    InputDeviceHandler::setFocus(Window *window)
>  
> ```

## 窗口管理

Wayland/Weston 窗口管理 窗口管理涉及的内容：

窗口堆栈的改变，focus的变化等

与安卓对比：

1、Wayland/Weston 没有安卓所谓的AMS（Activity的创建与管理、进程的创建与管理）

-------> 可见，窗口管理，才是必须的

2、窗口堆栈： 对于安卓，没有，只有Activity的堆栈（究其根因：安卓是单窗口系统）；对于 Wayland，是有的窗口堆栈

```
        ------->  linux窗口管理中的（窗口堆栈），一部分被AMS承担了
```

3、窗口的切换：

窗口管理的操作（如窗口堆栈的改变，focus的改变）

https://blog.csdn.net/a8039974/article/details/122867167

https://blog.csdn.net/qqzhaojianbiao/article/details/129790817?spm=1001.2014.3001.5502     Wayland窗口系统

## 输入Input

参考：

1、 https://blog.csdn.net/qqzhaojianbiao/article/details/130932031      KWin事件总结和相关类介绍    ------->  好文

2、收藏图

主要类：

> TouchInputRedirection

input.cpp

```
 InputEventFilter
 InputKeyboardFilter
```

## 输入法：

### 虚拟键盘florence

参考:

> https://www.xmodulo.com/onscreen-virtual-keyboard-linux.html    virtual keyboard
>
> --------> 验证ok，可以输入（仅限英文）

启动：

> florence

**结构：**

> florence是一个单独进程，类似service   ------>  目前手动执行命令启动（不同于安卓）

一些基本情况：

> florence 目前是最好的 + 开源的    https://www.linuxlinks.com/best-free-open-source-virtual-keyboards/
>
> kde环境下可运行

### 虚拟键盘Onboard

### 其他：没有虚拟键盘的

> https://www.jianshu.com/p/563b1f8aff97
>
> https://medium.com/@damko/a-simple-humble-but-comprehensive-guide-to-xkb-for-linux-6f1ad5e13450

kde下应用如何支持输入法？

> https://planet.kde.org/weng-xuetian-2023-01-06-how-to-make-your-application-support-input-method-under-linux/    How to make your application support Input method under Linux？
>
> ```
>  1、Create a connection to input method service.
>  2、Tell input method, you want to communicate with it.
>  3、Keyboard event being forwarded to input method
>  4、input method decide how key event is handled.
>  5、Receives input method event that carries text that you need to show, or commit to the application.6、
>  6、Tell input method you are done with text input
>  7、Close the connection when your application ends, or the relevant widget destructs.
> ```
>
> ---------------> 流程跟安卓一模一样！**必然，结构决定的**
>
> 不一样的点：linux侧这些是APP做的，安卓是IMMS做的

### 一些结论

从代码来看，kwin依赖于QT

QT源码下载： https://mirrors.tuna.tsinghua.edu.cn/qt/archive/qt/5.15/5.15.0/single/

参考： https://www.cnblogs.com/kn-zheng/p/17689855.html

## 参考：

https://blog.csdn.net/qqzhaojianbiao   好文：

```
 KWin事件总结和相关类介绍
 Wayland中跨进程调用过程
 Weston中shm window渲染
 Wayland窗口系统
```

https://blog.csdn.net/qqzhaojianbiao/article/details/129734727?spm=1001.2014.3001.5502    Weston介绍