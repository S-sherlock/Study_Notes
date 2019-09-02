## Python Thread join()用法

Thread 提供了让一个线程等待另一个线程完成的 join() 方法。当在某个程序执行流中调用其他线程的 join() 方法时，调用线程将被阻塞，直到被 join() 方法加入的 join 线程执行完成。

join() 方法通常由使用线程的程序调用，以将大问题划分成许多小问题，并为每个小问题分配一个线程。当所有的小问题都得到处理后，再调用主线程来进一步操作。



代码小例子如下

```python
import threading
# 定义action函数准备作为线程执行体使用
def action(max):
    for i in range(max):
        print(threading.current_thread().name + " " + str(i))
# 启动子线程
threading.Thread(target=action, args=(100,), name="新线程").start()
for i in range(100):
    if i == 20:
        jt = threading.Thread(target=action, args=(100,), name="被Join的线程")
        jt.start()
        # 主线程调用了jt线程的join()方法，主线程
        # 必须等jt执行结束才会向下执行
        jt.join()
    print(threading.current_thread().name + " " + str(i))
```

输出片段为

```
被Join的线程 97
被Join的线程 98
被Join的线程 99
MainThread 20
MainThread 21
MainThread 22
MainThread 23
MainThread 24
MainThread 25

```

从输出我们可以看到, 本来开始时主线程就启动了名为"新线程"的子线程, 然后这个子线程和主线程并发执行, 直到变量i=20时,启动了"被join的线程", 由于该线程调用了join()方法, 所以它不会和主线程并发运行, 必须它运行完之后 主线程才能运行. 在"被join的线程"执行时,实际上只有两个线程在跑,主线程在阻塞状态.



## python守护线程

守护线程又被称为“后台线程（Daemon Thread）”, 它是在后台运行的，它的任务是为其他线程提供服务, [Python](http://c.biancheng.net/python/) 解释器的垃圾回收线程就是典型的后台线程。

后台线程有一个特征，如果所有的前台线程都死亡了，那么后台线程会自动死亡。

调用 Thread 对象的 daemon 属性可以将指定线程设置成后台线程。下面程序将指定线程设置成后台线程，可以看到当所有的前台线程都死亡后，后台线程随之死亡。

```python
import threading

# 定义后台线程的线程执行体与普通线程没有任何区别
def action(max):
    for i in range(max):
        print(threading.current_thread().name + "  " + str(i))
t = threading.Thread(target=action, args=(100,), name='后台线程')
# 将此线程设置成后台线程
# 也可在创建Thread对象时通过daemon参数将其设为后台线程
t.daemon = True
# 启动后台线程
t.start()
for i in range(10):
    print(threading.current_thread().name + "  " + str(i))
# -----程序执行到此处，前台线程（主线程）结束------
# 后台线程也应该随之结束
```

输出

```
后台线程  0
后台线程  1
MainThread  0后台线程  2

后台线程  3
MainThread  1
MainThread  2
MainThread  3
后台线程  4
MainThread  4
MainThread  5
MainThread  6
后台线程  5
后台线程  6
后台线程  7
MainThread  7
后台线程  8
后台线程  9
后台线程  10
后台线程  11
后台线程  12MainThread  8
MainThread  9
```

可以看出来, 本来该线程应该执行到 i 等于 99 时才会结束，但是因为主线程运行到10就结束了，该后台线程也无法运行到 99,随着主程序的退出, 后台线程也就结束了.

**当前台线程死亡后，Python 解释器会通知后台线程死亡，但是从它接收指令到做出响应需要一定的时间。如果要将某个线程设置为后台线程，则必须在该线程启动之前进行设置。也就是说，将 daemon 属性设为 True，必须在 start() 方法调用之前进行，否则会引发 RuntimeError 异常。**