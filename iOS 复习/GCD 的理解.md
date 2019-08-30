## sync 和 async的区别
- 是否需要等待队列的任务执行完毕。sync 需要等待、async不需要等待。
- 是否具备开启新线程的能力。 sync不具备开启线程的能力、async则具备。

## 串行队列 与 并行队列 区别
队列都符合FIFO（先进先出）原则。
- 串行队列：在当前线程中执行，并且任务都是按顺序执行，任务1执行完毕之后才开始执行任务2.
- 并行队列：可并发执行任务，即任务2 无需等待任务1执行完毕即可执行。队列只保证了调用的顺序是FIFO的,但是无法保证的任务执行完成的先后顺序。

## 队列的获取
- 一般队列：dispatch_queue_create  
    - 队列名称一般使用 应用程序 ID 这种逆序全程域名
    - 串行 DISPATCH_QUEUE_SERIAL
    - 并行 DISPATCH_QUEUE_CONCURRENT 

- 主队列：dispatch_get_main_queue
- 全局并发队列：dispatch_get_global_queue
    - 第一个参数表示队列优先级，一般用DISPATCH_QUEUE_PRIORITY_DEFAULT。
    - 第二个参数暂时没用，用0即可。

## GCD 的基本使用
- sync与串行队列
    - 没有开启新线程，任务一个一个在当前线程执行。
- sync与并发队列：
    - 没有开启新线程，任务按顺序在当前线程中执行。
- sync与主队列:
    - 任务被放进了主队列，在主线程中执行。
- async与串行队列
    - 开启了新的线程，但因为是串行队列，所以只开启了一个。
- async与并发队列
    - 开启了新的线程，数量根据实际情况决定。
- async与主队列：
    - 没有开启新的线程，任务被放到了主队列，所以是在主线程中执行。
- 注意：
    - sync(同步)没有开启新线程的能力。
    - async(异步)拥有开启新线程的能力。
    - 主队列中的任务只会在主线程中执行。
    - 在主线程中同步执行主队列中的内容会造成死锁。但是放到其他线程中执行则不会。

- 使用NSThread 的detchNewThreadSelector 方法可以创建新的线程,自动启动线程执行。
```javascript
[NSThread detachNewThreadSelector:@selector(syncMain) toTarget:self withObject:nil];
```

## GCD 线程间的通信
```objective-c
dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
    dispatch_async(dispatch_get_main_queue(), ^{
        
    });
});
```

## GCD 的其他方法

### GCD 栅栏方法 dispatch_barrier_async 和  dispatch_barrier_sync
 
场景，两个异步任务组需要分开，并在异步执行完第一个任务组时执行第二个任务组。

- dispatch_barrier_async 和  dispatch_barrier_sync 的区别
    - dispatch_barrier_async 代表当执行到栅栏方法时，在栅栏中的任务是异步执行的，即另开了一个线程。后面的任务组无需等待栅栏任务执行完毕。
    - 而dispatch_barrier_sync 则是同步执行，后面的方法需要进行等待。
- 注意
    - 栅栏方法的使用条件：只能是使用dispatch_create_queue 方法执行创建的并发队列。 在串行队列和全局并发队列中等同于同步执行。

### GCD 延时执行方法：dispatch_after
- dispatch_after 只是在规定时间内后将任务添加到主队列中，并不意味着立即执行。
- 样例：
```
dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
    
    });
```

### GCD 只执行一次代码：dispatch_once
- 创建单例或者代码只需运行一次时用到。
- 在多线程环境下也是安全的。
```
- (void)once {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        // 只执行1次的代码(这里面默认是线程安全的)
    });
}
```

### GCD 快速迭代方法：dispatch_apply
- dispatch_apply按照指定的次数将指定的任务追加到指定的队列中，并等待全部队列执行结束。
- 提供快速迭代的能力，但是只在并行队列内有效，在串行队列中则是同步执行，与普通的for循环没有差别。
- apply函数在并行队列时，内部是异步执行的，即可开启其他的线程。
- apply 会等待函数中所包含的所有任务执行完毕才继续执行。

```
dispatch_queue_t concurrentQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);

dispatch_apply(5, concurrentQueue, ^(size_t index) {
    NSLog(@"dispatch_apply index = %@ currentThread = %@", @(index),[NSThread currentThread]);
});
```


### GCD 队列组：dispatch_group

- 创建：dispatch_group_create()
- 异步队列组：dispatch_group_async ：将任务添加到队列组中
- 通知：dispatch_group_notify ， 监听 group 中任务的完成状态，当所有的任务都执行完成后，追加任务到 group 中，并执行任务。
- dispatch_group_wait: 会堵塞当前线程，当group中的任务执行完毕后才会继续往下执行。
- dispatch_group_enter：标志着一个任务追加到 group。
- dispatch_group_leave：标志着 从group中删除呢一个任务。
- 当 group 中未执行完毕任务数为0的时候，才会使dispatch_group_wait解除阻塞，以及执行追加到dispatch_group_notify中的任务。


### Dispatch Semaphore 线程同步
- 创建： dispatch_semaphore_create
- 信号量增加1：dispatch_semaphore_signal
- 信号量减少1：dispatch_semaphore_wait
- 规则：信号量小于1时会堵塞当前线程，当信号量大于等于0时可以继续执行。
```
- (void)testSemaphore
{
    dispatch_semaphore_t semaphore = dispatch_semaphore_create(0);
    NSLog(@"cuurentThread = %@", [NSThread currentThread]);
    
    __block NSInteger number = 0;
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        for (int i = 0; i < 10; i++)
        {
            NSLog(@"currentThread = %@", [NSThread currentThread]);
        }
        
        number = 100;
        
        dispatch_semaphore_signal(semaphore);
    });
    
    dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER);
    
    NSLog(@"dispatchSemaphore end , number = %@", @(number));
}
```





## 注意
- Dispatch Queue 的名称推荐使用应用程序 ID 这种逆序全程域名
- 所有放在主队列中的任务，都会放到主线程中执行。

