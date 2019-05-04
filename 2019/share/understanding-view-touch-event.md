# Android view事件分发机制

本文内容大多来自任玉刚《Android开发艺术探索》的**View事件分发机制**小节。

所谓的点击事件的分发，其实就是对MotionEvent事件的分发过程，即当一个MotionEvent产生后，系统需要把这个事件传递给一个具体的View，而这个传递过程就是分发过程。点击事件的分发过程主要由三个很重要的方法来共同完成：`dispatchTouchEvent`、`onInterceptTouchEvent`和`onTouchEvent`。

```
public boolean dispatchTouchEvent(MotionEvent ev)
```
用来进行事件分发。如果事件能够传递给当前的View，那么此方法一定会被调用，返回结果受当前View的onTouchEvent和下级View的dispatchTouchEvent方法的影响，表示是否消耗当前事件。

```
public boolean onInterceptTouchEvent(MotionEvent ev)
```
在上述方法内部调用，用来判断是否拦截某个事件，如果当前View拦截了某个事件，那么在同一个事件序列当中，此方法不会被再次调用，返回结果表示是否拦截当前事件。

```
public boolean onTouchEvent(MotionEvent)
```
在dispatchTouchEvent方法中调用，用来处理点击事件，返回结果表示是否消耗当前事件，如果不消耗，则在同一个事件序列中，当前View无法再次接收到事件。

使用伪代码表示上述上个方法的关系：

```Java
public boolean dispatchTouchEvent(MotionEvent ev) {
    boolean consume = false;
    if (onInterceptTouchEvent(ev)) {
        consume = onTouchEvent(ev);
    } else {
        consume = child.dispatchTouchEvent(ev);
    }
    
    return consume;
}
```
从伪代码我们大致了解点击事件的传递规则：对于一个根ViewGroup来说，当点击事件产生后，首先会传递给它，这时它的dispatchTouchEvent就会被调用，如果这个ViewGroup的onInterceptTouchEvent方法返回true表示它要拦截当前事件，接着事件就会交给这个ViewGroup处理，即它的onTouchEvent事件会被调用；如果这个ViewGroup的onInterceptTouchEvent方法返回false，这时当前事件会继续传递给它的子元素，接着子元素的dispatchTouchEvent方法就会被调用，如此
反复直到事件被最终处理。

当然对于一个view而言，点击事件的优先级是： onTouch() > onTouchEvent() > onClick()

当一个点击事件产生后，它的传递过程是：Activity -> Window -> View，即事件总是先传递给Activity，Activity再传递给Window，最后Window再传递给顶级View。顶级View接收到事件后，就会按照事件分发机制去分发事件。
