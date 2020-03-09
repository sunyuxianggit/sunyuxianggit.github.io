# Python 进程和线程学习笔记

- ### 进程和线程概述

  进程：对于操作系统来说，一个任务就是一个进程（Process）

  线程：在一个进程内部，要同时干多件事，就需要同时运行多个“子任务”，我们把进程内的这些“子任务”称为线程（Thread）。

- ### 进程

  ```
  multprocessing # Python 中的 multiprocess 包提供了多进程支持
  
  ```
<!-- more -->
  ```
  #process multprocessing包中的一个类表示进程对象
  
  from multiprocessing import Process
  from tqdm import tqdm
  import os
  
  # 子进程要执行的代码
  def run_proc(name):
      print('Run child process %s (%s)...' % (name, os.getpid())) #getpid()可以拿到进程的ID。
      for i in tqdm(range(10000000)):
          pass
  
  if __name__=='__main__': 
  
  '''
  有化部分 ，这句代码以上的部分，可以被其它的调用，以下的部分只有这个文件自己可以看见，如果文件被调用了，其他人是无法看见私有化部分的
  也就是说你自己运行该模块的时候 这句话是执行的 因为自己运行时__name__就是__main__，而当别人调用你这个模块时，以下代码会被忽略，此时的__name__是模块名
  '''
      print('Parent process %s.' % os.getpid())
      p = Process(target=run_proc, args=('test1',))
      d = Process(target=run_proc, args=('test2',))
      print('Child process will start.')
      p.start()#调用进程
      d.start()#调用进程
      p.join()#join()方法可以等待子进程结束后再继续往下运行，通常用于进程间的同步
      d.join()
      print('Child process end.')
  ```

  

  ```
  #Pool multprocessing包中的一个类，如果要启动大量的子进程，可以用进程池的方式批量创建子进程：
  
  from multiprocessing import Pool
  import os, time, random
  # 子进程要执行的代码
  def long_time_task(name):
      # for i in tqdm(range(10000000)):
      #     pass
      print('Run task %s (%s)...' % (name, os.getpid()))
      start = time.time()
      time.sleep(random.random() * 3)
      end = time.time()
      print('Task %s runs %0.2f seconds.' % (name, (end - start)))
  
  if __name__=='__main__':
      print('Parent process %s.' % os.getpid())
      p = Pool(4)
      #创建子进程池
      #参数数决定同时运行多少进程 如果是4 task4会等待 0 1 2 3 运行完在运行，如果是5 就0 1 2 3 4 一起运行
      #如果你的参数大于你的CPU线程数还是要等待
      #把参数去掉，就是按照操作系统的核数来
      
      for i in range(13):
          p.apply_async(long_time_task, args=(i,)) #注意这里，因为是类所有调用函数是 P.
      print('Waiting for all subprocesses done...')
      p.close()
      p.join()
      print('All subprocesses done.')
  ```

  ```
  #这种方法可以实现任意进程间的通信，这里写的是主、子进程间的通信
  import multiprocessing
  
  def foo(aa):#必须要接收一个元祖
      message = aa.get()  # 管子的另一端放在子进程这里，子进程接收到了数据
      print('子进程已收到数据...')
      print(message)  # 子进程打印出了数据内容...
  
  
  
  if __name__ == '__main__': 
  
      xt = multiprocessing.Queue()  # 创建进程通信的Queue，你可以理解为我拿了个管子来...
      jc = multiprocessing.Process(target=foo, args=(xt,))  # multiprocessing.Process创建子进程
      jc.start()  # 启动子进程
      print('主进程准备发送数据...')
      xt.put('有内鬼，终止交易！')  # 将管子的一端放在主进程这里，主进程往管子里丢入数据
      jc.join()
  ```

- ### 线程

  ```
  启动一个线程就是把一个函数传入并创建Thread实例，然后调用start()开始执行：
  import time, threading
  # 新线程执行的代码:
  def loop():
      print('thread %s is running...' % threading.current_thread().name)
      n = 0
      while n < 5:
          n = n + 1
          print('thread %s >>> %s' % (threading.current_thread().name, n))
          time.sleep(1)
      print('thread %s ended.' % threading.current_thread().name)
  
  print('thread %s is running...' % threading.current_thread().name)
  t = threading.Thread(target=loop, name='LoopThread')
  t.start()
  t.join()
  print('thread %s ended.' % threading.current_thread().name)
  ```

  

