#Android Tips
### 事件传递
1. 事件从 `Activity.dispatchTouchEvent()`开始传递，只要没有被停止或拦截，从最上层的 View(ViewGroup)开始一直往下(子 View)传递。子 View 可以通过 `onTouchEvent()`对事件进行处理。

2. 事件由父 View(ViewGroup)传递给子 View，ViewGroup 可以通过 `onInterceptTouchEvent()`对事件做拦截，停止其往下传递。

3. 如果事件从上往下传递过程中一直没有被停止，且最底层子 View 没有消费事件，事件会反向往上传递，这时父 View(ViewGroup)可以进行消费，如果还是没有被消费的话，最后会到 Activity 的 `onTouchEvent()`函数。

4. 如果 View 没有对 ACTION_DOWN 进行消费，之后的其他事件不会传递过来。

5. OnTouchListener 优先于 onTouchEvent()对事件进行消费。

上面的消费即表示相应函数返回值为 true。比如:
``` java
    public boolean onTouchEvent(MotionEvent event) {
        if (mWindow.shouldCloseOnTouch(this, event)) {
            finish();
            return true;
        }
        
        return false;
    }
```

