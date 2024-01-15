

# profiler之 火焰图：

## 为什么要有火焰图？

systrace不能用嘛？ ----------->  systrace没有每一个函数的调用时间（只是打标签的一段）



## 获取

参考： 

 https://developer.android.google.cn/studio/profile/generate-trace-logs#java           Generate Trace Logs by Instrumenting Your App

https://developer.android.google.cn/studio/profile/record-traces   Record traces

https://developer.android.google.cn/studio/profile/inspect-traces             Inspect traces  分析

https://zhuanlan.zhihu.com/p/511224713    android studio profiler 性能分析   --------->  **好文**

https://www.jianshu.com/p/596b2ef68342       性能优化工具（十一）-Android Profiler    ---->  **结构**

## 分析 之 性能问题点：

> 平顶山：  火焰图就是看顶层的哪个函数占据的宽度最大。只要有"平顶山"，就表示该函数可能存在性能问题。
>
> 倒T 型： 如果A方法本身就慢呢？通过火焰图也是可以看出来的，这种底层栈的宽度很宽，但是建立在其撒花姑娘的调用链线条都很窄，火焰图呈现“┻”型，那么我们基本可以确定，栈底方法本身就存在性能问题。

https://www.qinglite.cn/doc/468764776cec22698









## 参考：





## linux火焰图 



https://www.qinglite.cn/doc/468764776cec22698     调优利器-火焰图使用图鉴

