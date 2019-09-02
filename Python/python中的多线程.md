# python中的多线程

Python 主要通过两种方式来创建线程：

1. 使用 threading 模块的 Thread 类的构造器创建线程。
2. 继承 threading 模块的 Thread 类创建线程类。

---



## 调用Thread 类的构造器创建线程

通过 Thread 类的构造器创建井启动多线程的步骤如下：

1. 调用 Thread 类的构造器创建线程对象。在创建线程对象时，target 参数指定的函数将作为线程执行体。
2. 调用线程对象的 start() 方法启动该线程。

```python

import threading
import time
# 定义一个普通的action函数，该函数准备作为线程执行体
t1 = time.time()
def action(max):
    for i in range(max):
        # 调用threading模块current_thread()函数获取当前线程
        # 线程对象的getName()方法获取当前线程的名字
        print(threading.current_thread().getName() +  " " + str(i))
# 下面是主程序（也就是主线程的执行体）
for i in range(100):
    # 调用threading模块current_thread()函数获取当前线程
    print(threading.current_thread().getName() +  " " + str(i))
    if i == 20:
        # 创建并启动第一个线程
        t1 =threading.Thread(target=action,args=(100,))
        t1.start()
        # 创建并启动第二个线程
        t2 =threading.Thread(target=action,args=(100,))
        t2.start()
print('主线程执行完成!')
t2 = time.time()
print('程序运行了{}'.format(t2-t1))
```

上面是一个多线程的小例子, 它其实一共运行了三个线程, 除了两个自定义的线程之外, 还有主程序也算一个线程, 我加了一个计算时间的代码, 但是它的输出是这样的. 

```cmd
Thread-1 60
Thread-2 97
Thread-2 98
Thread-2 99
Traceback (most recent call last):
  File "E:/Python_Notes/test.py", line 29, in <module>
Thread-1 61
    print('程序运行了{}'.format(t2-t1))
Thread-1 62
TypeError: unsupported operand type(s) for -: 'float' and 'Thread'
Thread-1 63
Thread-1 64

MainThread 98
MainThread 99Thread-1 65
Thread-1 66
```

线程1和线程2在交叉运行, 也就是说, 这三个线程的执行没有任何顺序, 谁拿到GIL谁执行, 典型的线程并发执行. CPU 以快速轮换的方式在多个线程之间切换，从而给用户一种错觉，即多个线程似乎同时在执行。甚至我加入的计算时间的, 都在中间被执行了.

- threading.current_thread()：它是 threading 模块的函数，该函数总是返回当前正在执行的线程对象。
- getName()：它是 Thread 类的实例方法，该方法返回调用它的线程名字。



---

## 继承Thread类创建线程类

推荐上面那种创建线程的方法, 这种没有上面那种清晰, 如果用的到再查文档吧!