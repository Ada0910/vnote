# 1. Sleep和wait
- sleep()是Thread类中的方法，sleep()方法导致了程序暂停，但是他的监控状态依然保持着，当指定的时间到了又会自动恢复运行状态。在调用sleep()方法的过程中，线程不会释放对象锁
- wait()则是Object类中的方法。
wait()方法会导致线程放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象调用notify()方法后本线程才进入对象锁定池准备获取对象锁进入运行状态。
注意是准备获取对象锁进入运行状态，而不是立即获得

- sleep()方法可以在任何地方使用；wait()方法则只能在同步方法或同步块中使用；
# 2. yield()没有参数

	sleep 方法使当前运行中的线程睡眠一段时间，进入不可以运行状态，这段时间的长短是由程序设定的，yield方法使当前线程让出CPU占有权，但让出的时间是不可设定的。

	yield()也不会释放锁标志。

	实际上，yield()方法对应了如下操作；先检测当前是否有相同优先级的线程处于同可运行状态，如有，则把CPU的占有权交给次线程，否则继续运行原来的线程，所以yield()方法称为“退让”，它把运行机会让给了同等级的其他线程

# 3. run和start()

把需要处理的代码放到run()方法中，start()方法启动线程将自动调用run()方法，这个由java的内存机制规定的。并且run()方法必需是public访问权限，返回值类型为void

