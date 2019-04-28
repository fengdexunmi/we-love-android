# Android系统启动过程

本篇文章参考了Gityuan的博客（http://gityuan.com/android/）和《Android进阶解密》


首先来看一张系统启动流程图，该图来自Gityuan的博客。

![android-boot](img/android-boot.jpg)

我们从`Loader`->`kernel`->`framework`->`Application`来分析Android系统启动流程。

## Loader
当按电源键开机的时，会引导芯片开始从固化在Rom的预设代码开始执行，然后加载引导程序到Ram。启动引导程序，把OS拉起来并运行。

## Kernel
内核启动，进行系统设置，完成后会寻找init文件，然后启动这个init进程。

## Native
这里Native系统库，主要包括Init孵化来的用户空间的守护进程、硬件抽象层（HAL）和开机动画。Init是Linix系统的守护进程，是所有用户空间进程的鼻祖。
- Init进程孵化uneventd、logd、adbd等用户守护进程
- Init进程会启动ServiceManager和开机动画等重要服务
- Init进程孵化Zygote进程

## FrameWork层
- Zygote进程，有Init进程通过解析init.rc文件后fork生成的。Zygote进程包含了
	- 加载ZygoteInit类，注册Zygote Socket服务端套接字
	- 加载虚拟机
	- 预加载类preloadClasses
	- 预加载资源preloadResources
- System Server进程，有Zygote进程孵化而来，System Server是Zygote孵化的第一个进程，System Server负责启动和管理整个Java FrameWork，包含Activity Manager、WindowManager、PackageManager、PowerManager等服务。

## App层

- Zygote进程孵化出的第一个App进程就是Launcher，即用户看到的桌面App
- 每一个App至少运行在一个进程上，所有App进程都是有Zygote进程fork生成。

## 从源码角度分析

待补充
