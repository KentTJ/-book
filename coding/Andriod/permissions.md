

# 问在前面（目的论）

权限： 

其实，权限状态是必然的。比如，做饭这件事情，

1、你要有出门的权限（疫情就不行），去买菜

2、房间要满足厨房的通风要求

3、生火的权限

.....................





权限的功能是啥？

----》1、如何理解应用权限 与 INTERNAL_SYSTEM_WINDOW 啥关系？ 

2、权限的本质是什么？ A申明了权限，B使用权限？

​     权限有哪些状态？（xml中申明？xml请求？用户同意？自定义？运行时？） ----->   软件必须弄懂状态



从信息角度：

> 限制来源于谁？限制于谁？



# 安卓权限概念

参考:

https://blog.csdn.net/sinat_20059415/article/details/80370223



见下  TODO









# 哪些app属于system app？

https://blog.csdn.net/Liu1314you/article/details/77368585

大体认识：

privileged app（特权app） >  System app >  

状态判定标准：

​          ApplicationInfo.FLAG_SYSTEM`

​           ApplicationInfo.PRIVATE_FLAG_PRIVILEGED



# 权限的声明&使用&获取

**声明权限**是指在AndroidManifest.xml中使用了`<permission>`， -----》 TODO: 不懂

**使用权限**是指在AndroidManifest.xml中使用了`<uses-permission>`

**获得权限（或赋予权限）**是指真正的可以<font color='red'>通过</font>系统的权限检查，调用到权限保护的方法



App A中声明了权限PermissionA，App B中使用了权限PermissionA--------> B可以调用A？



# 环境中查看状态

注意，

1、其实，**与运行时无关**，因为PMS这些权限的状态，与运行时无关  ---->  安装包决定的

2、与应用是否打开也无关

## 以package 为base，查看一个包涉及的权限

```java
adb shell dumpsys package <包名>
```

以com.tencent.mm为例： adb shell dumpsys package com.tencent.mm

**文件结构如下：**

1、给出自定义的Permission

推论：与 declared permissions中的权限 应该一样  TODO

%accordion%点击展示%accordion%

```java
Permissions:
  Permission [com.tencent.mm.WAID_PROVIDER_WRITE] (7bc450):
    sourcePackage=com.tencent.mm // sourcePackage是权限声明的地方
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{632649 com.tencent.mm.WAID_PROVIDER_WRITE}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.permission.EXT_OPEN_APPBRAND_LAUNCHER_UI] (dc0b46f):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{ee0d57c com.tencent.mm.permission.EXT_OPEN_APPBRAND_LAUNCHER_UI}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.matrix.strategynotify] (aede05):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{ef83e5a com.tencent.mm.matrix.strategynotify}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.permission.MM_MESSAGE] (64e288b):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{623cd68 com.tencent.mm.permission.MM_MESSAGE}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.plugin.permission.READ] (c445581):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{debe626 com.tencent.mm.plugin.permission.READ}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.wear.message] (4f4c267):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{21c5814 com.tencent.mm.wear.message}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.ext.permission.SPORT] (c5fc8bd):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=normal
    perm=Permission{3203eb2 com.tencent.mm.ext.permission.SPORT}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.ext.permission.WRITE] (7811e03):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature|privileged
    perm=Permission{270e180 com.tencent.mm.ext.permission.WRITE}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.backtrace.warmed_up] (7c533b9):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{ecf93fe com.tencent.mm.backtrace.warmed_up}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.permission.C2D_MESSAGE] (1ed975f):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{8c295ac com.tencent.mm.permission.C2D_MESSAGE}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.matrix.permission.PROCESS_SUPERVISOR] (37c5275):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{88cf20a com.tencent.mm.matrix.permission.PROCESS_SUPERVISOR}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.permission.EXT_OPEN_APPBRAND_MY_COLLECTIONS] (37e4a7b):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{d996098 com.tencent.mm.permission.EXT_OPEN_APPBRAND_MY_COLLECTIONS}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.manual.dump] (34ca0f1):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{c1024d6 com.tencent.mm.manual.dump}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.vfs.broadcast] (19d1357):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{80fee44 com.tencent.mm.vfs.broadcast}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.plugin.permission.WRITE] (5f95b2d):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature
    perm=Permission{6c1b862 com.tencent.mm.plugin.permission.WRITE}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}
  Permission [com.tencent.mm.permission.MOVE_XLOG] (4758df3):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature|privileged
    perm=Permission{43faab0 com.tencent.mm.permission.MOVE_XLOG}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

Permissions:
  Permission [com.tencent.mm.ext.permission.READ] (93d7d29):
    sourcePackage=com.tencent.mm
    uid=10108 gids=null type=0 prot=signature|privileged
    perm=Permission{206f8ae com.tencent.mm.ext.permission.READ}
    packageSetting=PackageSetting{557bb4e com.tencent.mm/10108}

```



%/accordion%



2、给出涉及的Permission

%accordion%点击展示%accordion%

```java
Packages:
  Package [com.tencent.mm] (557bb4e): //下面针对于 com.tencent.mm
    userId=10108  // userId到底是啥？
    pkg=Package{e030746 com.tencent.mm}
    codePath=/data/app/com.tencent.mm-FtxVmfhJnuP-P7oGWPgRrw==
    resourcePath=/data/app/com.tencent.mm-FtxVmfhJnuP-P7oGWPgRrw==
    legacyNativeLibraryDir=/data/app/com.tencent.mm-FtxVmfhJnuP-P7oGWPgRrw==/lib
    primaryCpuAbi=arm64-v8a   // 包路径、资源路径、lib路径
    secondaryCpuAbi=null
    versionCode=2400 minSdk=23 targetSdk=29 //需要的安卓sdk版本
    versionName=8.0.38
    splits=[base]
    apkSigningVersion=1
    applicationInfo=ApplicationInfo{f07dea4 com.tencent.mm}
    flags=[ HAS_CODE ALLOW_CLEAR_USER_DATA LARGE_HEAP ] // TODO: flags是啥
    privateFlags=[ PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_RESIZEABLE_VIA_SDK_VERSION ALLOW_AUDIO_PLAYBACK_CAPTURE PRIVATE_FLAG_REQUEST_LEGACY_EXTERNAL_STORAGE HAS_DOMAIN_URLS PARTIALLY_DIRECT_BOOT_AWARE ]
    dataDir=/data/user/0/com.tencent.mm
    supportsScreens=[small, medium, large, xlarge, resizeable, anyDensity]
    usesOptionalLibraries: 
      org.apache.http.legacy
      com.google.android.maps
      com.sec.android.app.multiwindow
      com.miui.easygo
      com.hihonor.easygo
      com.here.android
      androidx.window.extensions
      androidx.window.sidecar
      androidx.camera.extensions.impl
      com.huawei.easygo
      soterkeystore
    usesLibraryFiles:
      /system/framework/org.apache.http.legacy.jar
    timeStamp=2023-06-28 21:50:04
    firstInstallTime=2023-06-28 21:50:28
    lastUpdateTime=2023-06-28 21:50:28
    installerPackageName=com.android.packageinstaller
    signatures=PackageSignatures{a4cc409 version:1, signatures:[962f5b7], past signatures:[]} //签名
    installPermissionsFixed=true // 安装权限 -----> 如果装上了，一定为true？
    pkgFlags=[ HAS_CODE ALLOW_CLEAR_USER_DATA LARGE_HEAP ]
    declared permissions:  // 【1】微信自定义的权限
      com.tencent.mm.plugin.permission.WRITE: prot=signature, INSTALLED
      com.tencent.mm.plugin.permission.READ: prot=signature, INSTALLED
      com.tencent.mm.permission.MM_MESSAGE: prot=signature, INSTALLED
      com.tencent.mm.permission.MOVE_XLOG: prot=signature|privileged, INSTALLED
      com.tencent.mm.ext.permission.READ: prot=signature|privileged, INSTALLED
      com.tencent.mm.ext.permission.WRITE: prot=signature|privileged, INSTALLED
      com.tencent.mm.ext.permission.SPORT: prot=normal, INSTALLED
      com.tencent.mm.wear.message: prot=signature, INSTALLED
      com.tencent.mm.WAID_PROVIDER_WRITE: prot=signature, INSTALLED
      com.tencent.mm.permission.EXT_OPEN_APPBRAND_LAUNCHER_UI: prot=signature, INSTALLED
      com.tencent.mm.permission.EXT_OPEN_APPBRAND_MY_COLLECTIONS: prot=signature, INSTALLED
      com.tencent.mm.matrix.strategynotify: prot=signature, INSTALLED
      com.tencent.mm.vfs.broadcast: prot=signature, INSTALLED
      com.tencent.mm.manual.dump: prot=signature, INSTALLED
      com.tencent.mm.backtrace.warmed_up: prot=signature, INSTALLED
      com.tencent.mm.matrix.permission.PROCESS_SUPERVISOR: prot=signature, INSTALLED
      com.tencent.mm.permission.C2D_MESSAGE: prot=signature, INSTALLED
    requested permissions: // 【2】已经请求的权限。。。1、一定包含install permissions (对于已经安装了的pkg)  2、Todo: 一定都是用户授权了吗？
      android.bluetooth.permissions.SHORTCUT_ACTION
      android.permission.CHANGE_WIFI_MULTICAST_STATE
      com.tencent.mm.plugin.permission.READ
      com.tencent.mm.plugin.permission.WRITE
      com.tencent.mm.permission.MM_MESSAGE
      com.huawei.authentication.HW_ACCESS_AUTH_SERVICE
      com.google.android.providers.gsf.permission.READ_GSERVICES
      android.permission.CHANGE_NETWORK_STATE
      android.permission.ACCESS_NETWORK_STATE
      android.permission.ACCESS_COARSE_LOCATION
      android.permission.ACCESS_FINE_LOCATION
      android.permission.CAMERA// 微信自然需要CAMERA权限
      android.permission.GET_TASKS
      android.permission.INTERNET
      android.permission.MODIFY_AUDIO_SETTINGS
      android.permission.RECEIVE_BOOT_COMPLETED
      android.permission.RECORD_AUDIO
      android.permission.READ_CONTACTS
      android.permission.VIBRATE
      android.permission.WAKE_LOCK
      android.permission.WRITE_EXTERNAL_STORAGE: restricted=true
      com.android.launcher.permission.INSTALL_SHORTCUT
      com.android.launcher.permission.UNINSTALL_SHORTCUT
      com.android.launcher.permission.READ_SETTINGS
      com.tencent.mm.location.permission.SEND_VIEW
      android.permission.BLUETOOTH
      android.permission.BLUETOOTH_ADMIN
      android.permission.BROADCAST_STICKY
      android.permission.SYSTEM_ALERT_WINDOW
      android.permission.CHANGE_WIFI_STATE
      android.permission.GET_PACKAGE_SIZE
      android.permission.DOWNLOAD_WITHOUT_NOTIFICATION
      android.permission.NFC
      com.miui.easygo.permission.READ_PERMISSION
      com.huawei.android.launcher.permission.CHANGE_BADGE
      android.permission.WRITE_APP_BADGE
      cn.cyberidentity.certification.AUTH
      com.tencent.mm.ext.permission.READ
      com.tencent.mm.ext.permission.WRITE
      android.permission.ACTIVITY_RECOGNITION
      com.tencent.mm.wear.message
      android.permission.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS
      miui.permission.READ_STEPS
      android.permission.FOREGROUND_SERVICE
      android.permission.READ_EXTERNAL_STORAGE: restricted=true
      android.permission.ACCESS_WIFI_STATE
      com.open.gallery.smart.Read
      com.open.gallery.smart.Write
      com.open.gallery.smart.Provider
      android.permission.REQUEST_INSTALL_PACKAGES
      android.permission.USE_FULL_SCREEN_INTENT
      android.permission.USE_FINGERPRINT
      android.permission.USE_BIOMETRIC
      com.tencent.mm.WAID_PROVIDER_WRITE
      com.bbk.launcher2.permission.READ_SETTINGS
      com.android.vending.BILLING
      com.android.vending.CHECK_LICENSE
      com.tencent.mm.matrix.strategynotify
      android.permission.ACCESS_NOTIFICATION_POLICY
      com.tencent.mm.vfs.broadcast
      com.tencent.mm.manual.dump
      com.tencent.mm.backtrace.warmed_up
      com.tencent.mm.matrix.permission.PROCESS_SUPERVISOR
      com.huawei.easygo.permission.READ_PERMISSION
      com.oplus.permission.safe.FANTASYWINDOW
      com.google.android.finsky.permission.BIND_GET_INSTALL_REFERRER_SERVICE
      com.google.android.c2dm.permission.RECEIVE
      com.tencent.mm.permission.C2D_MESSAGE
      com.soter.permission.ACCESS_SOTER_KEYSTORE
      android.permission.USE_FACERECOGNITION
    install permissions: // 【3】安卓时，所必要的权限，安装上，必然用户授权 ranted=true
      android.permission.DOWNLOAD_WITHOUT_NOTIFICATION: granted=true
      android.permission.MODIFY_AUDIO_SETTINGS: granted=true
      android.permission.ACCESS_NOTIFICATION_POLICY: granted=true
      com.tencent.mm.WAID_PROVIDER_WRITE: granted=true
      com.tencent.mm.matrix.strategynotify: granted=true
      android.permission.NFC: granted=true
      android.permission.CHANGE_NETWORK_STATE: granted=true
      android.permission.FOREGROUND_SERVICE: granted=true
      android.permission.RECEIVE_BOOT_COMPLETED: granted=true
      com.tencent.mm.permission.MM_MESSAGE: granted=true
      com.android.launcher.permission.UNINSTALL_SHORTCUT: granted=true
      android.permission.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS: granted=true
      android.permission.BLUETOOTH: granted=true
      android.permission.CHANGE_WIFI_MULTICAST_STATE: granted=true
      android.permission.GET_TASKS: granted=true
      android.permission.INTERNET: granted=true
      android.permission.BLUETOOTH_ADMIN: granted=true
      android.permission.GET_PACKAGE_SIZE: granted=true
      com.tencent.mm.plugin.permission.READ: granted=true
      com.tencent.mm.wear.message: granted=true
      com.tencent.mm.ext.permission.WRITE: granted=true
      android.permission.USE_FULL_SCREEN_INTENT: granted=true
      android.permission.BROADCAST_STICKY: granted=true
      android.permission.CHANGE_WIFI_STATE: granted=true
      com.tencent.mm.backtrace.warmed_up: granted=true
      android.permission.ACCESS_NETWORK_STATE: granted=true
      com.tencent.mm.permission.C2D_MESSAGE: granted=true
      android.permission.USE_FINGERPRINT: granted=true
      com.tencent.mm.matrix.permission.PROCESS_SUPERVISOR: granted=true
      com.tencent.mm.manual.dump: granted=true
      com.tencent.mm.vfs.broadcast: granted=true
      com.tencent.mm.plugin.permission.WRITE: granted=true
      android.permission.VIBRATE: granted=true
      android.permission.ACCESS_WIFI_STATE: granted=true
      android.permission.USE_BIOMETRIC: granted=true
      com.android.launcher.permission.INSTALL_SHORTCUT: granted=true
      android.permission.WAKE_LOCK: granted=true
      com.tencent.mm.ext.permission.READ: granted=true
    User 0: ceDataInode=1106633 installed=true hidden=false suspended=false stopped=false notLaunched=false enabled=0 instant=false virtual=false
      gids=[3002, 3003, 3001]
      runtime permissions: // 【4】运行时的权限：运行时，才申请？？？？所以很多用户还没有授权granted=false
        android.permission.ACCESS_FINE_LOCATION: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
        android.permission.READ_EXTERNAL_STORAGE: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED|RESTRICTION_INSTALLER_EXEMPT]
        android.permission.ACCESS_COARSE_LOCATION: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
        android.permission.CAMERA: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
        android.permission.WRITE_EXTERNAL_STORAGE: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED|RESTRICTION_INSTALLER_EXEMPT]
        android.permission.ACTIVITY_RECOGNITION: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
        android.permission.RECORD_AUDIO: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
        android.permission.READ_CONTACTS: granted=false, flags=[ USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
      disabledComponents:
        com.tencent.mm.plugin.nfc_open.ui.NfcWebViewUI
      enabledComponents:
        androidx.work.impl.background.systemjob.SystemJobService

```



%/accordion%

结论:  

> 见上



### declared permissions ：自定义权限

作用：

> 与其他应用分享自己的资源和功能

比如：启动 myapp 的activity  ----> 功能分享出去

代码：

> 1、分享侧自定义DEADLY_ACTIVITY权限：<permission>
>
> ```java
> //AndroidManifest.xml
> <manifest
>   xmlns:android="http://schemas.android.com/apk/res/android"
>   package="com.example.myapp" >
>     
>     <permission            android:name="com.example.myapp.permission.DEADLY_ACTIVITY"
>       android:label="@string/permlab_deadlyActivity"
>       android:description="@string/permdesc_deadlyActivity"
>       android:permissionGroup="android.permission-group.COST_MONEY"
>       android:protectionLevel="dangerous" />
>     ...
> </manifest>
> ```
>
> 2、调用侧 TODO
>
> 在AndroidManifest.xml中使用了`<uses-permission>`  ？？？



TODO:   <permission>  和  `<uses-permission>`  在dump中怎么体现的？





TODO: protectionLevel、permissionGroup

https://blog.csdn.net/hanhan1016/article/details/105864367



### requested permissions ：请求权限

### install permissions ：安装权限

### runtime permissions ： 运行时权限



## 以权限为base，查哪些声明了，哪些使用了

```java
adb shell dumpsys package permission <权限名>
```





%accordion%结果：%accordion%





```java
Permissions:
  Permission [android.permission.CALL_PHONE] (cb5d7a9):
    sourcePackage=android  // 权限的申明者？？？？
    uid=1000 gids=null type=0 prot=dangerous //保护级别
    perm=Permission{6d8812e android.permission.CALL_PHONE}
    packageSetting=PackageSetting{bb25476 android/1000}

Packages:  //下面是涉及到这个权限的所有pkg
  Package [com.android.providers.telephony] (788ac20): // 权限的使用者
    userId=1001
    sharedUser=SharedUserSetting{91f34cf android.uid.phone/1001}
    pkg=Package{b7abe5c com.android.providers.telephony}
    codePath=/system/priv-app/TelephonyProvider
    versionCode=29 minSdk=29 targetSdk=29
    versionName=10
    splits=[base]
    apkSigningVersion=3
    applicationInfo=ApplicationInfo{6b4565 com.android.providers.telephony}
    flags=[ SYSTEM HAS_CODE ALLOW_BACKUP KILL_AFTER_RESTORE RESTORE_ANY_VERSION ]
    privateFlags=[ PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_RESIZEABLE_VIA_SDK_VERSION ALLOW_AUDIO_PLAYBACK_CAPTURE BACKUP_IN_FOREGROUND DEFAULT_TO_DEVICE_PROTECTED_STORAGE DIRECT_BOOT_AWARE PRIVILEGED ]
    dataDir=/data/user_de/0/com.android.providers.telephony
    supportsScreens=[small, medium, large, xlarge, resizeable, anyDensity]
    timeStamp=2020-11-13 02:05:45
    firstInstallTime=2020-11-13 02:05:45
    lastUpdateTime=2020-11-13 02:05:45
    signatures=PackageSignatures{def823a version:3, signatures:[b4addb29], past signatures:[]}
    installPermissionsFixed=true
    pkgFlags=[ SYSTEM HAS_CODE ALLOW_BACKUP KILL_AFTER_RESTORE RESTORE_ANY_VERSION ]
    requested permissions:
    install permissions:
    User 0: ceDataInode=950283 installed=true hidden=false suspended=false stopped=false notLaunched=false enabled=0 instant=false virtual=false
  Package [com.android.dynsystem] (a6339d9):
    userId=1000
    sharedUser=SharedUserSetting{b844eeb android.uid.system/1000}
    pkg=Package{c236448 com.android.dynsystem}
    codePath=/system/priv-app/DynamicSystemInstallationService
    versionCode=29 minSdk=29 targetSdk=29
    versionName=10
    splits=[base]
    apkSigningVersion=3
    applicationInfo=ApplicationInfo{b4fd2e1 com.android.dynsystem}
    flags=[ SYSTEM HAS_CODE ALLOW_CLEAR_USER_DATA ]
    privateFlags=[ PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_RESIZEABLE_VIA_SDK_VERSION ALLOW_AUDIO_PLAYBACK_CAPTURE PRIVILEGED ]
    dataDir=/data/user/0/com.android.dynsystem
    supportsScreens=[small, medium, large, xlarge, resizeable, anyDensity]
    timeStamp=2020-11-13 02:14:46
    firstInstallTime=2020-11-13 02:14:46
    lastUpdateTime=2020-11-13 02:14:46
    signatures=PackageSignatures{e438806 version:3, signatures:[b4addb29], past signatures:[]}
    installPermissionsFixed=true
    pkgFlags=[ SYSTEM HAS_CODE ALLOW_CLEAR_USER_DATA ]
    requested permissions:
    install permissions:
    User 0: ceDataInode=950286 installed=true hidden=false suspended=false stopped=false notLaunched=false enabled=0 instant=false virtual=false

    Package [com.android.messaging] (656fd19):
    userId=10065
    pkg=Package{e7d91de com.android.messaging}
    codePath=/system/app/messaging
    versionCode=10001040 minSdk=29 targetSdk=28
    versionName=1.0.001
    splits=[base]
    apkSigningVersion=3
    applicationInfo=ApplicationInfo{d9cefbf com.android.messaging}
    flags=[ SYSTEM HAS_CODE ALLOW_CLEAR_USER_DATA ]
    privateFlags=[ PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_RESIZEABLE_VIA_SDK_VERSION PRIVATE_FLAG_REQUEST_LEGACY_EXTERNAL_STORAGE ]
    dataDir=/data/user/0/com.android.messaging
    supportsScreens=[small, medium, large, xlarge, resizeable, anyDensity]
    usesLibraries:
      android.hidl.manager-V1.0-java
      android.hidl.base-V1.0-java
    usesLibraryFiles:
      /system/framework/android.hidl.manager-V1.0-java.jar
      /system/framework/android.hidl.base-V1.0-java.jar
    timeStamp=2020-11-13 02:14:42
    firstInstallTime=2020-11-13 02:14:42
    lastUpdateTime=2020-11-13 02:14:42
    signatures=PackageSignatures{998768c version:3, signatures:[b4addb29], past signatures:[]}
    installPermissionsFixed=true
    pkgFlags=[ SYSTEM HAS_CODE ALLOW_CLEAR_USER_DATA ]
    requested permissions:
      android.permission.CALL_PHONE
    install permissions:
    User 0: ceDataInode=950337 installed=true hidden=false suspended=false stopped=false notLaunched=false enabled=0 instant=false virtual=false
      gids=[3003]
      runtime permissions:
        android.permission.CALL_PHONE: granted=true, flags=[ 
            // granted=true 证明该pkg已经获得了权限
 GRANTED_BY_DEFAULT|USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
            
    Package [com.android.dialer] (15fd569):
    userId=10085
    pkg=Package{10a0bee com.android.dialer}
    codePath=/system/product/priv-app/Dialer
    versionCode=2900000 minSdk=29 targetSdk=28
    versionName=23.0
    splits=[base]
    apkSigningVersion=3
    applicationInfo=ApplicationInfo{8fdc88f com.android.dialer}
    flags=[ SYSTEM HAS_CODE ALLOW_CLEAR_USER_DATA ALLOW_BACKUP ]
    privateFlags=[ PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_RESIZEABLE_VIA_SDK_VERSION PRIVATE_FLAG_REQUEST_LEGACY_EXTERNAL_STORAGE PARTIALLY_DIRECT_BOOT_AWARE PRIVILEGED PRODUCT ]
    dataDir=/data/user/0/com.android.dialer
    supportsScreens=[small, medium, large, xlarge, resizeable, anyDensity]
    usesLibraries:
      android.hidl.manager-V1.0-java
      android.hidl.base-V1.0-java
      org.apache.http.legacy
    usesLibraryFiles:
      /system/framework/android.hidl.manager-V1.0-java.jar
      /system/framework/android.hidl.base-V1.0-java.jar
      /system/framework/org.apache.http.legacy.jar
    timeStamp=2020-11-13 02:16:32
    firstInstallTime=2020-11-13 02:16:32
    lastUpdateTime=2020-11-13 02:16:32
    signatures=PackageSignatures{4c4271c version:3, signatures:[9542d9b9], past signatures:[]}
    installPermissionsFixed=true
    pkgFlags=[ SYSTEM HAS_CODE ALLOW_CLEAR_USER_DATA ALLOW_BACKUP ]
    requested permissions:
      android.permission.CALL_PHONE
    install permissions:
    User 0: ceDataInode=950512 installed=true hidden=false suspended=false stopped=false notLaunched=false enabled=0 instant=false virtual=false
      gids=[3002, 3003, 3001]
      runtime permissions:
        android.permission.CALL_PHONE: granted=true, flags=[ GRANTED_BY_DEFAULT|USER_SENSITIVE_WHEN_GRANTED|USER_SENSITIVE_WHEN_DENIED]
```



%/accordion%



dialer！！！



比如：android.permission.CALL_PHONE 权限，有了这个权限，可以调用打电话的APP(自然，它声明了)









TODO:

这个查看结果，使用户设置之后的嘛？









# 参考：

https://blog.csdn.net/Liu1314you/article/details/77368585  Android 权限的一些细节

https://developer.android.com/reference/android/Manifest.permission.html   ------>完整版的权限表

https://www.jianshu.com/p/6c755ca6a3a3  

https://blog.csdn.net/hanhan1016/article/details/105864367  Android权限 - 查看应用权限信息

https://www.cnblogs.com/andy-songwei/p/10638446.html    [Android安全之（一）权限篇](https://www.cnblogs.com/andy-songwei/p/10638446.html)

https://blog.csdn.net/li6151770/article/details/52782141   Android调用打电话(Call Phone)



# 格式

%accordion%点击展示%accordion%

隐藏



%/accordion%

