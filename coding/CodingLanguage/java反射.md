# 反射

## 为什么要用反射？精髓在哪里？

**安卓源码中反射的场景：**

> XML  ------>  类： new view()
>
>
> byte  -------->  类： 反序列化创建 parcelable，使用到的Creator   参考：[android对象序列化Parcelable浅析 - xerrard - 博客园 (cnblogs.com)](https://www.cnblogs.com/xerrard/p/5144386.html)          https://www.cnblogs.com/xerrard/p/5144386.html
>
--------------->  总之:  <font color='red'>（1）都是从二进制 到 类的过程   （2）一般都是很多个类-----> 反射，大量简化代码量</font>！！！！！！

注意，**安卓为了优化性能，缓存策略：**

> 安卓的Parcelable  mCreators，就用了缓存策略！！！！！！！！！ <String, Parcelable.Creator>   ！！！！！！
>
>
> 即：~~在真正binder通信时，没有反射~~
>

**从设计角度看Creator：**

> 只是把反序列化过程 要用的反射部分，从 parcelable 里抽出来
>
> ------------> 为啥要这样？
>
> （1）我们要用缓存策略，反射太耗时
>
> （2）不可能缓存 Parcelable 对象的，因为没一个对象都是不一样的，参数是parcel里的数据
>
> （3）所以，**<font color='red'>抽出公共部分，并缓存公共部分</font>：**（**太TM神了!!!!!**!）
>
> Parcelable 对象的创建部分，即 Creater（这个对于同一个类的Parcelable ）
>

**总结：**

> 1、缓存对象
>
> 2、<font color='red'>缓存不了对象，缓存对象的创建方式  Creater</font>
>
> 3、**Creater是永远不变的、静态的，不是对象**

-**<font color='red'>反射，可以实现的功能（或者  为啥要用反射）</font>：**

> （1）从编译角度，可以避开编译约束:   私有，不可见
>
>
> （2）从代码角度, 可以实现 **统一，大量简化代码量**（一行代码，可以new 出各种类）------->  **语言，架构层面**
>
> （3） 从功能角度：同（1）
>
>  (4)  从**代码 扩展性、弹性角度：**   反射有着非常好的弹性。比如，新增一个MyView，xml解析器创建view的代码，<font color='red'>不需要改动一行</font>

## 反射性能

https://blog.csdn.net/googdev/article/details/100039092#:~:text=%E4%B8%8B%E9%9D%A2%E6%98%AF%E6%88%91%E4%BB%AC%E7%9A%84%E6%B5%8B%E8%AF%95%E7%BB%93%E6%9E%9C%EF%BC%88%E6%97%B6%E9%97%B4%E5%8D%95%E4%BD%8D%E5%9D%87%E4%B8%BA%E6%AF%AB%E7%A7%92%EF%BC%89%EF%BC%9A,%E6%98%BE%E7%84%B6%EF%BC%8C%E5%9C%A8Android%E4%B8%AD%E4%BD%BF%E7%94%A8%E5%8F%8D%E5%B0%84%E9%9D%9E%E5%B8%B8%E6%85%A2%EF%BC%8C%E4%BD%BF%E7%94%A8%E5%8F%8D%E5%B0%84%E7%9A%84%E6%B5%8B%E8%AF%95%E6%89%A7%E8%A1%8C%E5%88%86%E5%88%AB%E9%9C%80%E8%A6%811332%E6%AF%AB%E7%A7%92%E3%80%816384%E6%AF%AB%E7%A7%92%E3%80%812891%E6%AF%AB%E7%A7%92%EF%BC%8C%E8%80%8C%E4%B8%8D%E4%BD%BF%E7%94%A8%E5%8F%8D%E5%B0%84%E5%88%99%E5%8F%AA%E9%9C%80%E8%A6%81312%E6%AF%AB%E7%A7%92%E3%80%81358%E6%AF%AB%E7%A7%92%E3%80%81774%E6%AF%AB%E7%A7%92%E3%80%82

https://www.skoumal.com/en/is-android-reflection-really-slow/

## 反射性能优化

1、主要很耗时的函数，有些函数不耗时：

Class.forName这个方法比较耗时 （它实际上调用了一个本地方法，通过这个方法来要求JVM查找并加载指定的类）

getFields

。。。。。。。。。。。。

2、优化-----------缓存策略：

```
 把Class.forName返回的Class对象缓存起来，下一次使用的时候直接从缓存里面获取，这样就极大的提高了获取Class的效率
 同理，在我们获取Constructor、Method等对象的时候也可以缓存起来使用
```

https://blog.51cto.com/u_16213578/7527733

TODO:  安卓的Parcelable  mCreators，似乎就用了缓存策略

3、关闭安全检查

通过setAccessible(true)的方式可以关闭安全检查：

> jdk在设置获取字段，调用方法的时候会执行安全访问检查，而此类操作会比较耗时，所以通过setAccessible(true)的方式可以关闭安全检查，从而提升反射效率
>

4、高性能反射工具包ReflectASM