# 目录

[TOC]



# **docker**

官方文档是学习一门新技术的灯塔

官方用户手册

https://docs.docker.com/docker-for-windows/#file-sharing

## ~~**docker**账号：~~

ID chen85
1519........

## 目前使用的版本

### images版本

每个版本增加什么功能！！！！！



### docker版本

原始安装版本以及更新后的版本：
![image-20220724161653731](Docker.assets/image-20220724161653731.png)

docker version



~~2021年10月：~~

 ![image-20221109224037856](Docker.assets/image-20221109224037856.png)





2022年11月：

![image-20221110005749324](Docker.assets/image-20221110005749324.png)





## **docker安装配置**

### Docker Desktop 安装

**分为WSL2方式和 Hyper-v方式**

注意：install required windows components for WSL 2   **不要勾选**

![image-20221109224528945](Docker.assets/image-20221109224528945.png)



原因在于：上面勾选了WSL 2, 后面 setting中配置 resources只能用  .wslconfig 文件来配置了

​                  1、不勾选 WSL 2   --->使用的是Hyper-V   ---> 界面配置：

![image-20221109225048661](Docker.assets/image-20221109225048661.png)

​				

  2、勾选  --->  使用的是WSL 2  --->    .wslconfig 文件配置



### <font color='red'> 安装后，修改docker的镜像目录 </font>

跟版本有关！！！！！！！

![img](Docker.assets/clipboard.png)

其他版本可能可以用：

mklink /J "C:\Users\Public\Documents\Hyper-V\Virtual Hard Disks"  E:\docker\Hyper-V



docker的配置文章：

https://www.it610.com/article/1290832596699258880.htm

https://www.runoob.com/docker/docker-mirror-acceleration.html

https://blog.csdn.net/whatday/article/details/86770609

https://www.cnblogs.com/brock0624/p/9792203.html

### 源的配置---> 可选

![image-20220912172121827](Docker.assets/image-20220912172121827.png)

```java
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "registry-mirrors": [
  "https://registry.docker-cn.com",
  "http://hub-mirror.c.163.com",
  "https://docker.mirrors.ustc.edu.cn"
  ],
  "debug": true,
  "experimental": true,
  "features": {
    "buildkit": true
  }
}
```

## 教程链接

https://yeasy.gitbook.io/docker_practice/image/build  docker入门到实践



https://docs.docker.com/engine/reference/commandline/image_build/  官方文档，包括所有命令参数
https://docs.docker.com/desktop/



## docker  代理配置



### 法一： Ubuntu内部设置代理 

（与宿主win无关）

```shell
docker run  ^
-p  127.0.0.1:10000:22  ^
-p 127.0.0.1:15901:5901  ^
--cap-add SYS_ADMIN --device /dev/fuse ^
-t -i  -v   I:\dockerSharedFiles:/home/cg, /workingSpace/local ^
-v   H:\dockerSharedFiles_Gpan\dockerSharedFiles_Gpan:/home/cg/workingSpace/local2    %imagesId%  /bin/bash
 
 
 
 //  --cap-add SYS_ADMIN --device /dev/fuse 作用：
 qv2ray会使用到fuse，见 
https://stackoverflow.com/questions/48402218/fuse-inside-docker
https://stackoverflow.com/questions/51263555/riofs-fuse-device-not-found
```



教程：<font color='red'>TODO:  完善自己的</font>

```
https://bwfish.xyz/archives/linux%E4%B8%AD%E5%8F%AF%E7%94%A8%E7%9A%84%E6%9C%BA%E5%9C%BA%E5%B7%A5%E5%85%B7qv2ray.
https://www.zsxcool.com/7137.html
```

#### 方法一步骤

##### 配置核心执行文件

![image-20221218110708766](Docker.assets/image-20221218110708766.png)



##### 订阅配置

![image-20221218115152333](Docker.assets/image-20221218115152333.png)

![QQ截图20220207133027](Docker.assets/S8dHMiZbsOuGhAC.png)



##### 最终结果：

![image-20221218114954741](Docker.assets/image-20221218114954741.png)

![image-20221120173835150](Docker.assets/image-20221120173835150.png)




注意：**方法一的关键点**：ping的时间在100ms左右。否则，打开Google非常卡，有时候打不开；Clion用不了

![image-20221120192030500](Docker.assets/image-20221120192030500.png)



##### 遇到的问题：

脚本执行后，卡在这个位置

![image-20221218114611928](Docker.assets/image-20221218114611928.png)



解决办法：新开一个terminal，

再次执行：

```shell
sudo ./Qv2ray-v2.7.0-linux-x64.AppImage

# if clinet UI does apper, try sudo ./Qv2ray-v2.7.0-linux-x64.AppImage again
```



有时候遇到界面没有跳出来，需要手动点开：

![image-20230707000916699](Docker.assets/image-20230707000916699.png)



#### 基于方法一，给Ubuntu配置系统全局代理---一个终端(临时)

todo： 是不是clion和浏览器就不用配置了？-----> 测试浏览器不是

参考：Ubuntu设置全局socks代理  https://blog.csdn.net/weixin_45033342/article/details/118075525?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-1-118075525-blog-128040047.pc_relevant_3mothn_strategy_recovery&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-1-118075525-blog-128040047.pc_relevant_3mothn_strategy_recovery&utm_relevant_index=2

```
export ALL_PROXY='socks5://127.0.0.1:1089'

//验证
curl http://www.baidu.com
curl http://www.google.com
curl https://www.google.com

取消设置的代理
unset ALL_PROXY
```

![image-20221120190657401](Docker.assets/image-20221120190657401.png)

#### 基于方法一，给Ubuntu配置系统全局代理---持久化



\# 配置代理
编辑vi  ~/.bashrc，增加代理配置  ---->  <font color='red'>持久化</font>

```cpp
export http_proxy="127.0.0.1:8889"
export https_proxy="127.0.0.1:8889"
    
或
export ALL_PROXY='socks5://127.0.0.1:1089'
```

**注意：**
此处在bashrc中配置了HTTP代理，但它可能会影响那些需要访问本地网络的bash命令行工具的使用。譬如：kubernetes集群客户端工具kubectl。在开启代理的情况下，kubectl无法顺利连接到APIServer，需要unset http_proxy和https_proxy两个环境变量，才可恢复正常。



#### 基于方法一，Ubuntu下浏览器链接代理

![image-20221218004112916](Docker.assets/image-20221218004112916.png)

![image-20221120191310971](Docker.assets/image-20221120191310971.png)

#### 基于方法一，Ubuntu下clion链接代理



![image-20221120190306304](Docker.assets/image-20221120190306304.png)

### 法二： 宿主win给 Ubuntu共享代理网络

 ----> TODO:  不知道怎么用的





## **docker命令**

### **docker启动：**

docker images -a



暂时不用：
docker run -t -i    eca1757c6d5c   /bin/bash   

### **挂载磁盘**

主机F:\shareFiles挂载到/home/chen目录下

docker run -t -i  -v   F:\shareFiles:/home/chen/workingSpace/local   f2bab1877e15    /bin/bash

前提：

![img](Docker.assets/clipboard-1605373146599.png)



 <font color='red'> 下面命令，包括：挂载，以及ssh </font> ------------>常用链接

需修改： <font color='red'> 13874e71186c </font>

docker run   -p   10000:22   -t -i  -v    G:\dockerSharedFiles:/home/chen/workingSpace/local  -v     H:\dockerSharedFiles_Gpan\dockerSharedFiles_Gpan:/home/chen/workingSpace/local2       403c641a0264 /bin/bash
---->目前：10000需要改成20000



注意：需设置

![img](Docker.assets/clipboard-1605373411779.png)



### ssh链接

关于ssh链接：

> https://blog.csdn.net/Leo_csdn_/article/details/96150534?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.channel_param
>
> https://blog.csdn.net/qq_37955980/article/details/83044482   docker学习之ssh连接
>
> https://blog.csdn.net/vincent2610/article/details/52490397?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param   ssh远程连接docker中的container
>

主机端口：10000    <-----上面命令导致的



**使用MobaXterm远程连接步骤：**

（1）进入容器并重新开启ssh  ---->   每次都需要

[root@68e7598797d7 /]  #         /usr/sbin/sshd -D &

![image-20210306200055100](Docker.assets/image-20210306200055100.png)

（2）使用MobaXterm远程连接

通过xshell或者其他连接工具进行远程连接了，**记住，ip是宿主机的IP地址，端口号宿主机上的端口，**=即上面端口映射命令中的10000 ！

注意:主机ip很容易变动！！！！

![image-20201115011406102](Docker.assets/image-20201115011406102.png)

MobaXterm设置：框内是主机的ip，即是IPV4

![img](Docker.assets/clipboard-1605373856628.png)



### 账号

user：cheng

密码：151937Cgkent.

root账号密码：151937Cgkent.

注意：<font color='red'>最初cmd窗口就是root</font>
![image-20220109204739135](Docker.assets/image-20220109204739135.png)

### docker commit（禁止使用）

docker ps -l

![img](Docker.assets/1605374123405.png)

docker commit  7a96e91   newName: v2

![img](Docker.assets/1605374190736.png)

Ubuntu镜像

规定提交的格式为：
1、 docker tag 1be049d70e93  chen85/ubuntu16.04_aosp1000_r17:v1
--->这个是docker hub push识别的格式
2、或：docker commit  7a96e91 chen/ubuntu16.04_aosp1000_r17:v1



**禁止原因：**

> `docker commit` 制作镜像，以及后期修改的话，每一次修改都会让镜像更加臃肿一次，所删除的上一层的东西并不会丢失，会一直如影随形的跟着这个镜像，即使根本无法访问到。这会让镜像更加臃肿

### Dockerfile 定制镜像 

docker build使用 Dockerfile 定制镜像

---> 防止image臃肿



```c
#H:\docker_anzhuang2\Dockerfile
FROM chen85/ubuntu16.04_aosp1000_r17:v1
RUN echo '这是一个本地构建的nginx镜像' > /home/chen/index.html
```

![image-20220731115615101](Docker.assets/image-20220731115615101.png)

在 `Dockerfile` 文件所在目录执行：

```
 docker build -t chen85/ubuntu16.04_aosp1000_r17:v2 .
```

#### 关于Dockerfile详细说明：

概念：

![img](Docker.assets/modb_20220429_cfcd8976-c752-11ec-82c6-38f9d3cd240d.png)

技巧：用一个RUN命令替代多个 ---> 产生一个层

```shell
FROM debian:stretch

RUN apt-get update
RUN apt-get install -y gcc libc6-dev make wget
RUN wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz"
RUN mkdir -p /usr/src/redis
RUN tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1
RUN make -C /usr/src/redis
RUN make -C /usr/src/redis install
```

正确：

```shell
FROM debian:stretch

RUN set -x; buildDeps='gcc libc6-dev make wget' \
    && apt-get update \
    && apt-get install -y $buildDeps \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz" \
    && mkdir -p /usr/src/redis \
    && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
    && make -C /usr/src/redis \
    && make -C /usr/src/redis install \
    && rm -rf /var/lib/apt/lists/* \
    && rm redis.tar.gz \
    && rm -r /usr/src/redis \
    && apt-get purge -y --auto-remove $buildDeps
```


到这一组命令的最后添加了清理工作的命令，清理了所有下载、展开的文件，并且还清理了apt 缓存文件





### 合并镜像层 squash



```c
--squash    -->  Squash newly built layers into a single new layer
```

优点：1、把Image多层变成一层，合并多个commit，有效减小体积！

```cpp
 docker build --squash -t chen85/ubuntu16.04_aosp1000_r17:v4 .
```

--->默认使用了Dockerfile，需要修改

![image-20221121005402258](Docker.assets/image-20221121005402258.png)

 2、docker history  076f7dad0851 信息并没有减



查看层数：

![image-20220731145005969](Docker.assets/image-20220731145005969.png)



-<font color='red'>注意： squash 能生效是有前提的：</font>

至少：

```java
  "experimental": true,

    "buildkit": true
```

![image-20230915004242984](Docker.assets/image-20230915004242984.png)



对应设置界面：





### Docker 容器连接的优化：

原始：

```shell
docker run  ^
-p  10000:22  ^
-p 15901:5901  ^
-t -i  -v   G:\dockerSharedFiles:/home/cg, /workingSpace/local ^
-v   H:\dockerSharedFiles_Gpan\dockerSharedFiles_Gpan:/home/cg, /workingSpace/local2    4753178ab18f  /bin/bash
```

MobaXterm链接时的ip使用主机的：
![image-20221101185509314](Docker.assets/image-20221101185509314.png)

对应运行的

![image-20221101185358120](Docker.assets/image-20221101185358120.png)



**优化：**

```shell
docker run  ^
-p  127.0.0.1:10000:22  ^
-p 127.0.0.1:15901:5901  ^
-t -i  -v   G:\dockerSharedFiles:/home/cg, /workingSpace/local ^
-v   H:\dockerSharedFiles_Gpan\dockerSharedFiles_Gpan:/home/cg, /workingSpace/local2    4753178ab18f  /bin/bash
```

MobaXterm链接时的ip使用主机的127.0.0.1，<font color='red'>固定！！！！</font>

`相当于有一层代理，具体使用时，不用管具体ip是多少`

![image-20221101185718576](Docker.assets/image-20221101185718576.png)

对应运行的

![image-20221101185635895](Docker.assets/image-20221101185635895.png)



### 远端

#### docker账号登录：docker login

chen85

#### docker push

```
docker push tag 1be049d70e93  chen85/ubuntu16.04_aosp1000_r17:v1
```

见：https://www.cnblogs.com/kevingrace/p/9599988.html    [通过容器提交镜像（docker commit）以及推送镜像（docker push）笔记](https://www.cnblogs.com/kevingrace/p/9599988.html)  下载





### 根据镜像id修改镜像tag名

修改镜像tag：

```java
docker tag $IMAGE_ID <NEW_IMAGE_NAME>:<NEW_TAG>
    
docker tag 194d8716 cg, /ubuntu16.04_aosp1200_r28:last
```



### docker网站，

类似gitHub

```
https://hub.docker.com/
```

### 下载别人的镜像

```c
docker pull wangshibo/myubuntu:v1
```

### **镜像保存成jar到本地**

https://blog.csdn.net/qq_37797234/article/details/103660748

保存镜像到本地！！！！

```shell
docker save  ubuntu16.04_0902  -o  E:\docker\ubuntu16.04_0902.jar
docker save  cg, /ubuntu16.04_aosp1000_r17:vnc_ok  -o  H:\docker_jarFiles\ubuntu16.04_aosp1000_r17_vnc_20221026.jar
```

 加载本地镜像

```shell
C:\Users\xixi>docker load -i  F:\VirtualMachine\Docker\ubuntu.tar
```



#### 优化之 保存jar

规定（<font color='red'>优</font>）： 

1、在物理磁盘小的情况下，build+ 保存jar，先删除源码   --------> 节省最终存储jar的空间

2、保存jar时，如果出现no space。见《build或者保存jar，no space>







## 优化

### 配置修改优化

原则：

1、从**配置文件修改（优）**，而不是界面操作  < --------- 界面化操作需要先进入，然后重启

​                                                                                    没启动docker时，从配置文件修改，只需要启动一次

比如：

```java
"dataFolder": "D:\\programFiles\\dockerDesktop\\DockerDesktop",

```

![image-20230915003554954](Docker.assets/image-20230915003554954.png)

对应设置：

> ![image-20230915004242984](Docker.assets/image-20230915004242984.png)
>



对应设置界面：



## CPU使用率优化 **docker pause**

```
docker pause/unpause <container-id>
```

不用时，docker puase  ---> sleep 容器内所有进程

------> 极大的降低CPU使用率



## 文件系统过大优化



### 优化之  只保存系统文件到 jar

**规定：**

1、导出jar时，<font color='red'>只保存系统，不保存代码</font>       ------->  背后思想，提取重复

方法：squash过程中删除代码

![image-20230916003538219](Docker.assets/image-20230916003538219.png)



2、<font color='red'>代码+系统的演进，仍然用squash之前的</font>  ------>  **因为包含代码**

​     squash后的img导出jar后，就删除！



关系如图：

> 演进的始终是一个东西
>
> 版本只是一个时间点的快照

![image-20230916003820830](Docker.assets/image-20230916003820830.png)

大大减小了系统版本快照大小。且不影响 演进

![image-20231014030319612](Docker.assets/image-20231014030319612.png)

### 时间优化之 两份aosp同时解压

基于《优化之  只保存系统文件到 jar》 ---------> aosp用压缩包解压策略：

> 可以两份同时解压，但cpu也只用到了10%
>
> ![image-20231014025859380](Docker.assets/image-20231014025859380.png)

### 时间优化之 build squash 时，容器可用

实际上，只有commit时，容器不能用，其他都可以

------>  所以，基于容器角度，<font color='red'>结论：</font>

**build squash以及保存jar等，不影响我们的正常工作（在容器内）**

### 优化之  任何差异保存到window路径下----减少commit

-<font color='red'>极优</font>

目的：减少commit  



步骤：

> 1、 <font color='red'>把变化保存在windows里，不进行commit</font>
>
> 比如 .myfunction.sh  .my_start.sh
>
> 2、在启动时候，从win复制过去
>
> ![image-20230916012925941](Docker.assets/image-20230916012925941.png)



---------------------->  **此方法问题：**

如果改动在越来越多文件里扩散，就比较麻烦

**总结：**

> 在常用的文件里可以如此改动
>
> **减少commit，可以用squash方法替代**

### 镜像优化之合并镜像层 squash

方法见上

**<font color='red'>优点：</font>**

> 1、**压缩到一层**：减小
>
> 2、**独立性：** 压缩后的镜像不依赖其他镜像，其他可删

### docker rmi  删除镜像

方法一：通过imageID： docker rmi       ee7cbd482336



删除镜像难以删除的情况：
1、一个ImageID 对应多个   repositories：

![image-20220731152848039](Docker.assets/image-20220731152848039.png)

![image-20220731152939515](Docker.assets/image-20220731152939515.png)

删除方法（<font color='red'>**优**</font>）：  `注意：不能在cmd下运行`

```
 docker rmi chen85/ubuntu16.04_aosp1000_r17:v1
```



2、<font color='red'>被正在运行的容器关联了</font>： container 368027944f2e is using its referenced image
![image-20220731153210469](Docker.assets/image-20220731153210469.png)

方法：删除所有容器，再删image

```shell
 docker stop $(docker ps -a -q)   #全关
 docker rm $(docker ps -a -q)   #删除全部容器
 docker rmi ubuntu16.04_aosp1000_r17:latest   #删除image
```



### **磁盘overlay满了**  docker system prune  

docker system prune 
 ----> <font color='red'>已经验证，十分有用</font>： 清除不用的镜像和<font color='red'>缓存</font>

https://www.cnblogs.com/wswang/p/10736726.html



例子：

> 
>
> ![image-20231014022142307](Docker.assets/image-20231014022142307.png)
>
> docker system prune  之后：
>
> ![image-20231014023947830](Docker.assets/image-20231014023947830.png)
>
> ![image-20231014024658623](Docker.assets/image-20231014024658623.png)

（3)删？？？？‪

![img](Docker.assets/clipboard-1605374444293.png)

(2)

https://segmentfault.com/q/1010000020545464

https://www.jianshu.com/p/9174914ec07d



### img优化 &  DockerDesktop.vhdx优化 --------><font color='red'> docker的最大难题</font>

img大小（即保存的jar大小） ------ >  可以由squash优化压缩

-**DockerDesktop.vhdx 大小  一般会大于 最大img大小（即使只有一个img）** <font color='red'> 根本原因：</font> .vhdx 大小  是<font color='red'>历史上最大</font>的文件系统（即使后面删除很多文件，也不会减小）



-------------------------> 针对于docker这些特性：  

（1）在linux侧，不保存差异

（2）差异保存在win侧：**每次启动**的  img都一样，DockerDesktop.vhdx 大小 也都一样（即**<font color='red'>保证永远不会恶化</font>**）

​      即： <font color='red'>重启一致性</font>

（3）**观察df  -h，avail的存储指标**。小，则会报错no space

![image-20230927034334031](Docker.assets/image-20230927034334031.png)

（4）**优化df  -h**：  长时间没有关闭linux，这个很容易被用完。`利用重启一致性，来恢复avail 区域大小`

​     ---------------> <font color='red'> 这反而成为docker的优点</font>

### 优化之 复制 DockerDesktop.vhdx----->save jar的保底

目的：

1、尽管减少commit，但是随着演进，img越来越大。最终导致磁盘太小，<font color='red'>无法save jar，需要挪盘----->save jar的保底</font>

2、做备份



![image-20230916084702048](Docker.assets/image-20230916084702048.png)

-<font color='red'>移动文件需要everyone权限</font>

> ![image-20220722223854271](Docker.assets/image-20220722223854271.png)
>

### 优化之  保存jar 与 进入容器使用 并行

这个两个 可以并行：docker 与 容器内，可以并行

-----> 节省时间



### 积木化思想

ubuntu纯净系统： 

>   ----->  squash时，删除aosp代码
>
> 优点： 保存的jar包很小

经常变化的文件：

> 存储于win文件系统。开机后，sync过去

aosp代码：

> 过大，可以以zip压缩形式，存储于win。然后解压到linux中



------------------->  总结：

积木化思想



### 总结所有优化

docker容器内文件系统过大表现：

> （1）容器内部看：overlay used很大
>
> ![image-20231014022839601](Docker.assets/image-20231014022839601.png)
>
> （2）从win来看：
>
> ![image-20231014022928865](Docker.assets/image-20231014022928865.png)

-------------------->   overlay used<font color='red'>很大根因：</font>

> （1）文件系统包括了 aosp代码   -------->  没必要保存，squash时+ 保存jar 时，删掉
>
> （2）即使没有aosp代码，有时候也很大  ------>  因为<font color='red'>历史镜像最大值</font> + <font color='red'>缓存</font>造成的
>
> ​                                     a. squash压缩得到一个新的独立镜像 ------>  <font color='red'>因为独立，可以删除其他所有镜像</font>
>
> ​                                     b. docker system prune 清除缓存
>
> ​      -<font color='red'>技巧：</font>
>
> > ​     先删除大文件，比如aosp，然后docker system prune 清除缓存





时间优化， 总结： 

> 1、
>
> 2、
>
> 



## 技巧之保存 每个特性代码





## error

### ~~**docker报错:**图标变红  方法1：~~

cmd报错：open //./pipe/docker_engine: The system cannot find the file specified.

解决方法：  

> 重置配置，，，，再重启电脑
>
> ![img](Docker.assets/clipboard-1605374501667.png)
>

~~reset to factory defaults~~   ---->  `规定：后面不准用这种方法，会造成setting改变`


### 启动异常报错-->方法2:

运行docker出现System.InvalidOperationException错误

![image-20210714075640590](Docker.assets/image-20210714075640590.png)



解决办法：
1.管理员方式运行cmd
2.输入命令：netsh winsock reset
![在这里插入图片描述](Docker.assets/20210126212610700.png)

3.重新启动Docker
4.重新switch to Linux Containers
5.成功

 **技巧，特别注意：**
此时，应该所有的image都没有了。尽量不要重新load image，应该把这个文件复制到setting目录下面

![image-20220109195305006](Docker.assets/image-20220109195305006.png)

![image-20220109195422495](Docker.assets/image-20220109195422495.png)



移动文件需要everyone权限

![image-20220722223854271](Docker.assets/image-20220722223854271.png)

### 启动异常报错  方法3:（优）

~~reset to factory defaults仍然报错~~   ---->  `规定：后面不准用这种方法，会造成setting改变`

--->技巧：不要重新装docker软件
关闭docker，重启win，删除DockerDesktop.vhdx，启动docker ---->docker会自动生成

优点：**不用重新装docker，setting不会变**

### 启动异常报错  方法4:（极优）

1、备份好的DockerDesktop.vhdx

2、基于方法3，恢复环境

3、copy  .vhdx至目录下，重启    ---->  优化了load  jar的时间



TODO: 以后备份都用.vhdx？ 而不是jar？



### build或者保存jar，no space

报错：

```java
Error response from daemon: write /var/lib/docker/tmp/docker-export-1172560351/26512da3e9ed0583440e8dee1123b7548cef5b82a5cc9954ba36e354a 3db6e57/layer.tar: no space left on device
```

![image-20230915223615835](Docker.assets/image-20230915223615835.png)



问题根因：

> 这里给docker的空间小了（相比于已经存在的img）
>
> ![image-20230915224728576](Docker.assets/image-20230915224728576.png)
>
> ![image-20230915224716660](Docker.assets/image-20230915224716660.png)

办法：

> 设置扩大diskSizeMiB
>
> -------------->  如果物理磁盘不够了，就挪动到其他盘。方法，见《docker优化之 复制 DockerDesktop.vhdx>





参考：

https://devpress.csdn.net/cloud-native/645cf29477f0ca41cb317090.html?dp_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6NDUyNjk2LCJleHAiOjE2OTUzMTIzMDQsImlhdCI6MTY5NDcwNzUwNCwidXNlcm5hbWUiOiJ3ZWl4aW5fMzgxOTkzODEifQ.xIBvzCJnk72YDqUIB6k1ZsDN6yx0IlPq-bCs7XSvrr0 

## docker连 主机pc连的安卓设备

1、宿主为windows：
https://testerhome.com/topics/8049     https://blog.csdn.net/qq_42693848/article/details/109505849
         使用方法

​        **宿主windows侧：**起服务

1.  安装 nodejs

    推荐使用 nvm 安装： http://events.jianshu.io/p/5db570f53e6a      `win10中使用nvm安装nodejs遇到的坑整理`    ------> ==很好的教程==

2.  安装 adbkit

    ```
    $ npm i -g adbkit
    ```

3.  启动 usb 转 tcp 服务

    ```
    $ adbkit usb-device-to-tcp -p $port $serial
    // serial是手机序列号
    
    比如：adbkit usb-device-to-tcp -p 7788 FA6930305260
    ```

    **docker侧：**连接服务

4.  在docker侧（装有 adb 的机器上）进行设备的连接

    假设启动 adbkit 服务的机器 ip 是 10.0.0.1 端口是 15555 ----> ip试了HOST的真实ip，以及

    ```
    $ adb connect 10.0.0.1:7788
    ```

​                 -<font color='red'>万能方法</font>：  为了方便docker提供了专门的标志表示母机的ip：host.docker.internal
​           见：https://blog.csdn.net/qq_42693848/article/details/109505849

```CPP
adb connect host.docker.internal:$port

 adb connect host.docker.internal:7788  
```

  **其他注意事项**

adb连接手机默认使用的是5037，母机与容器交互的接口无法在使用该端口，需要选择其他端口。

 

2、宿主linux：https://stackoverflow.com/questions/29563183/connecting-to-a-usb-android-device-in-a-docker-container-via-adb



## docker容器安装图形桌面

VNC远程win： tigervncserver -vncClient



参考： https://blog.csdn.net/lxyoucan/article/details/121679346

视频教程：https://www.bilibili.com/video/BV1mL41177Av



**总之，使用**：

```shell
 docker run  ^
 -p  10000:22  ^
 -p 15901:5901  ^
 -t -i  -v   G:\dockerSharedFiles:/home/chen/workingSpace/local ^
 -v   H:\dockerSharedFiles_Gpan\dockerSharedFiles_Gpan:/home/chen/workingSpace/local2   8ea41f5aa2f9 /bin/bash
 
:: /usr/sbin/sshd -D &
::  -p  10000:22  用于ssh登录 --->使用win ip  + 10000 ： 192.168.1.4:10000 
:: -p 15901:5901  用于VNC界面登录 --->使用win 的Ethernet adapter ip  + 15901: 172.18.28.1:15901
                                                                   或  127.0.0.1::15901 ----> 永恒不变
```



**注意点：**

1、关于vnc链接的ip ： <font color='red'>使用win 的Ethernet adapter ip</font>

![image-20221026004429896](Docker.assets/image-20221026004429896.png)

![image-20221026004536110](Docker.assets/image-20221026004536110.png)

登录：
![image-20221026011422272](Docker.assets/image-20221026011422272.png)



![image-20221026004338396](Docker.assets/image-20221026004338396.png)



### 问题1：VNC连不上的

报错：connection closed
操作方法：

```shell
// 1、重启一下服务试试
vncserver -kill :1
vncserver -kill :2
vncserver -kill :3
vncserver -kill :4
vncserver :1 -localhost no -geometry=1920x1080


// 2、如果上述不行，看有没有提示：
Warning: 4fcb1df56031:1 is taken because of /tmp/.X1-lock   ---->删掉这个文件
Remove this file if there is no X server 4fcb1df56031:1
```

![image-20221026225716419](Docker.assets/image-20221026225716419.png)



### 关于VNC 客户端的选择

1、TurboVNC Viewer（<font color='red'>优</font>）

![image-20230107001518272](Docker.assets/image-20230107001518272.png)

可以双屏



全屏切换快捷键   ALT + Enter



2、集成了TurboVNC Viewer的 MobaXterm

~~有些问题：必须全屏 + stay on TOP  有些快捷键才生效~~； 复制粘贴 偶现问题

![image-20230107001356052](Docker.assets/image-20230107001356052.png)



## 剪切板复制 docker与win

https://zhuanlan.zhihu.com/p/518188941    VNC 复制粘贴中文乱码问题及解决方案



https://unix.stackexchange.com/questions/35030/how-can-i-copy-paste-data-to-and-from-the-windows-clipboard-to-an-opensuse-clipb

![image-20221028232748568](Docker.assets/image-20221028232748568.png)

结合两个帖子：

```
vim ~/.vnc/xstartup

#加入：
# win --> docker
vncconfig  -nowin &

vncconfig -iconic &
autocutsel -fork&
```



注意：

1、重启一个vnc服务



### 报错与解决办法

vnc:   failed to open clipboard    https://blog.csdn.net/qq_39590599/article/details/116228624

![image-20221127105159908](Docker.assets/image-20221127105159908.png)

![img](Docker.assets/202104281044288.png)





## 一些环境规定：

`docker 命令必须在bash环境下执行`，不能用cmd（除了启动的）





## 关于docker网络的几个概念区分：

1、使用的网络类型是bridge，桥接的宿主网络(组成局域网)
（1）在容器外使用时：
moba连接时，用的127.0.0.1，为啥？ ----->   因为原先用的宿主的ip，宿主ip对于寄生的docker就是127.0.0.1
（2）在容器内使用时：
必须用宿主机的绝对ip，不能是127.0.0.1？----->   因为在内部，相对ip172是其自己
2、使用--network=host，这样的话使用 -p 参数映射端口就没用了， 即所有端口都是开放和宿主机共享的
（2）在容器内使用时：
能用127.0.0.1  -----> 因为127是自己 ，也是host，共享的端口



代理软件：
本地 关系：AS对win是本地的关系; AS对docker 容器
           moba对win是本地的关系
		   
局域网关系：
  docker内部对win

 

## 优化

自动保存 DockerDesktop.vhdx 脚本  ：vhdx_backup.bat   --->  **不影响docker  容器unbuntu正常工作的，可以白天经常做**

自动build、save、load脚本：docker_build_save_load.bat    ---> 必须晚上






