# WMS动画系统

分类：

> 窗口动画
>
> 过渡动画
>
> 屏幕旋转动画

TODO: 区别

# Android 动画原理

Android中动画的工作过程：
在某一个时间点，调用getTransformation()，根据mStartTime和mDuration，**计算出当前的进度，**在根据mInterpolator计算出转换的进度，然后计算出属性的当前值，保存在matrix中。 再调用Matrix.getValues将属性值取出，运用在动画目标上。

## Animation 和 Transform

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1becffecc3f6446084da6de642f79b95~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

Animation ：

​           给定了初始状态、结束状态、启动时间与持续时间  ------>  **用于计算 Transformation**

Transformation：

​             Transformation 描述了一个变换，即：透明度和一个二维变换矩阵



补充Animation 子类：

> 子类：TranslateAnimation，ScaleAnimation，RotateAnimation，AlphaAnimation

## Choreographer

VSync信号驱动动画



# 窗口动画



# 过渡动画



# 屏幕旋转动画

参考：https://juejin.cn/post/7026611124482670600

触发点（自然时机）： 

​       屏幕旋转<font color='red'>之后</font>   注意：此时动画还没开启

代码触发点：

> TODO: 具体哪个模块？
>
> OrientationListener#onProposedRotationChanged
>
> ​         -------> WMS#updateRotation



## 动画之图片

动画启动的最基本元素

来源， 截图：

```java
// ScreenRotationAnimation

final SurfaceControl.Transaction t = mService.mTransactionFactory.get();
SurfaceControl.LayerCaptureArgs args =
    new SurfaceControl.LayerCaptureArgs.Builder(displayContent.getSurfaceControl())
    .setCaptureSecureLayers(true)
    .setAllowProtected(true)
    .setSourceCrop(new Rect(0, 0, mWidth, mHeight))
    .build();
SurfaceControl.ScreenshotHardwareBuffer screenshotBuffer =
    SurfaceControl.captureLayers(args);

// 这一大段就是抓取截图绑定到mScreenshotLayer
GraphicBuffer buffer = GraphicBuffer.createFromHardwareBuffer(
    screenshotBuffer.getHardwareBuffer());
t.setBuffer(mScreenshotLayer, buffer);
```



承载的surface：

```java
 // mScreenshotLayer是真正用于旋转动画的surface           
mScreenshotLayer = displayContent.makeOverlay()
```





setRotationTransform 中  mScreenshotLayer承载的数据，被旋转
        t.setPosition(mScreenshotLayer, x, y);
        t.setMatrix(mScreenshotLayer,
                mTmpFloats[Matrix.MSCALE_X], mTmpFloats[Matrix.MSKEW_Y],
                mTmpFloats[Matrix.MSKEW_X], mTmpFloats[Matrix.MSCALE_Y]);

        t.setAlpha(mScreenshotLayer, (float) 1.0);
        t.show(mScreenshotLayer);
       




截图获取bitmap方法：

```java
final SurfaceControl.ScreenshotHardwareBuffer screenshotBuffer =
    SurfaceControl.captureDisplay(captureArgs);
screenshot = screenshotBuffer == null ? null : screenshotBuffer.asBitmap();

// 参考： https://blog.csdn.net/weixin_46297800/article/details/131939823


// 转成png
Bitmap bitmap = screenshotBuffer.asBitmap();
OutputStream os = new FileOutputStream("/sdcard/screen.png");
bitmap.compress(Bitmap.CompressFormat.PNG, 100, os);
os.flush();
os.close();
```



```
Bitmap bitmap = null;
Class<?> cls  = Class.forName("android.view.SurfaceControl");
Method method = cls.getMethod("screenshot", Rect.class, int.class, int.class, int.class);
Object obj = method.invoke(null, new Rect(0, 0, 500, 1000), 500, 1000, 0);
bitmap = (Bitmap) obj;
```





```
        String  name = "app2";
        File cacheDir = context.getCacheDir();
        Log.i("liuhongliang","cacheDir" + cacheDir.toString());
        cachePath = "/sdcard/" + name + ".png";
        String cmd = "screencap -p /sdcard/" + name + ".png";
        // 权限设置
        Process p = Runtime.getRuntime().exec("sh"); //Process process = rt.exec("su");
        // 获取输出流
        OutputStream outputStream = p.getOutputStream();
        DataOutputStream dataOutputStream = new DataOutputStream(
        outputStream);
        // 将命令写入
        dataOutputStream.writeBytes(cmd);
        // 提交命令
        dataOutputStream.flush();
        // 关闭流操作
        dataOutputStream.close();
        outputStream.close();
————————————————
版权声明：本文为CSDN博主「小刘不上班」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_46297800/article/details/131939823
```

TODO:  

**冻屏动画**或者**无缝动画**



## 横竖屏切换动画时间优化方案

https://zhuanlan.zhihu.com/p/265291842



## 旋转屏幕的一些认知

1、<font color='red'>横向永远是x</font>（即旋转后，x值更大）

2、更新surface位置   <------------>  布局窗口relayoutWindow  何时？

**最终逃不过的点，不得不：** 

> SurfaceControl.setPosition

```
setPosition:2756, SurfaceControl$Transaction (android.view)
lambda$new$1$WindowState:854, WindowState (com.android.server.wm)
accept:-1, WindowState$$ExternalSyntheticLambda3 (com.android.server.wm)
updateSurfacePosition:5547, WindowState (com.android.server.wm)
updateSurfacePositionNonOrganized:3042, WindowContainer (com.android.server.wm)
prepareSurfaces:5491, WindowState (com.android.server.wm)
prepareSurfaces:2445, WindowContainer (com.android.server.wm)
prepareSurfaces:2445, WindowContainer (com.android.server.wm)
prepareSurfaces:2445, WindowContainer (com.android.server.wm)
prepareSurfaces:644, DisplayArea$Dimmable (com.android.server.wm)
prepareSurfaces:4879, DisplayContent (com.android.server.wm)
applySurfaceChangesTransaction:4335, DisplayContent (com.android.server.wm)
applySurfaceChangesTransaction:1068, RootWindowContainer (com.android.server.wm)
performSurfacePlacementNoTrace:844, RootWindowContainer (com.android.server.wm)
performSurfacePlacement:797, RootWindowContainer (com.android.server.wm)
performSurfacePlacementLoop:177, WindowSurfacePlacer (com.android.server.wm)  ------>  这里，所有
performSurfacePlacement:126, WindowSurfacePlacer (com.android.server.wm)
relayoutWindow:2386, WindowManagerService (com.android.server.wm)
relayout:235, Session (com.android.server.wm)
onTransact:735, IWindowSession$Stub (android.view)
onTransact:169, Session (com.android.server.wm)
execTransactInternal:1184, Binder (android.os)
execTransact:1143, Binder (android.os)
```





![image-20230825013906246](wms_animination.assets/image-20230825013906246.png)



# 参考



# 抛开动画代码



# 动画背后的思想