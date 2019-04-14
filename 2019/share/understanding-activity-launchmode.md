# Activity启动模式

参考**任玉刚的《Android开发艺术探索》**

在默认情况下，当我们启动Activity的时候，系统会创建实例并把它放入任务栈。当我们按下back键，Activity会从栈中退出。任务栈是“后进先出”的栈结构。

Activity启动模式分为四种：
- standard
- singleTop
- singleTask
- singleInstance

## standard
标准启动模式，即是默认情况，每次启动一个Activity都会重新创建一个实例，不管这个实例是否已经存在任务栈中。被创建的实例的onCreate()、onStart()、onResume()方法都会被调用。在这种模式下，谁启动了这个Activity，那么这个Activity就运行在启动它的那个Activity所在的栈中。

## singleTop
栈顶复用模式，如果要启动的Activity已经在任务栈的栈顶，那么此Activity不会被重新创建，而是复用栈顶的Activity，但是它的onCreate()、onStart()不会被调用，它的`onNewInent()`方法会被调用。如果要启动的Activity实例已经存在但是不在栈顶，则此Activity会被重新创建。

## singleTask
栈内复用模式，如果要启动的Activity已经在栈内，则不会重新创建此Activity的实例。它的onCreate()、onStart()不会被调用，它的`onNewInent()`方法会被调用。当启动一个singleTask模式的Activity A时，系统首先会寻找是否存在A想要的任务栈，如果不存在就会重新创建一个任务栈，把A放入到这个栈中。如果A想要的任务栈已经存在，系统会使该栈中在A上面的Activity出栈，这样A就会位于栈顶（即启动了A），并调用它的onNewIntent()方法。

## singleIntance
单实例模式，这是一种加强的singleTask模式，它具有singleTask的特性，第一次启动该模式的Activity，系统会为它创建一个任务栈，Activity单独位于这个任务栈中，后续请求启动这个Activity都会使用这个任务栈的Activity实例不会创建新的。除非这个任务栈被销毁了。
