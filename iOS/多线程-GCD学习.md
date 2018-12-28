## iOS多线程-GCD学习
### 入门须知
学习GCD主要需要理解两个概念， `任务&队列`。

### 任务 & 队列
1. 任务:**简单的说就是程序执行的一个代码片段**。任务的执行的方式有两种，`同步执行` 和 `异步执行`。
- **同步执行**
  - 同步执行的最大的特征是程序必须等待任务执行完成才能继续往下执行。 
  - 同步执行不具备创建新线程的能力，所以同步任务是在当前线程执行的。

- **异步执行**
  - 异步执行的最大的特征是程序不需要等待任务执行完成就能继续往下执行。 
  - 异步执行具备创建新线程的能力,通常的会和`并发队列`一起使用。 

2. 队列: **队列是任务暂时存放的地方，队列一般都遵循着先进先出(FIFO)的原则**。正常来说，队列分为有两种，`串行队列` 和 `并发队列`。
- **串行队列**
  - 串行队列的任务只在一个线程中执行， 并且任务按照先后顺序执行，且只有当前一个任务执行完毕后，才会执行下一个任务。 
  - 串行队列不具备开启线程的能力。
    ![Alt](/图片/串行队列.jpg)

- **并发队列**
  - 并发队列的任务也是按照先后顺序执行的，但是后面的任务不需等待前面的任务执行完成即可开始执行。
  - 并发队列具备开启多线程的能力。队列中的任务可以交过多个线程同时执行。
  ![Alt](/图片/并发队列.jpg)

- **主线程队列**
  - 主线程队列是一个比较特殊的串行队列。主线程队列中的任务都是在主线程中执行的。使用主线程队列需要注意的一个问题是`“死锁”`


3. **任务和队列的组合**
由上面可知，任务的执行方式和队列的组合可以划分为6种
    ```
    同步执行 + 并发队列
    异步执行 + 并发队列
    同步执行 + 串行队列
    异步执行 + 串行队列
    同步执行 + 主线程队列
    异步执行 + 主线程队列
    ```

-  同步执行 + 串行队列
```Objective-c
/* 同步串行队列*/
+ (void)testSynSerialQueue
{
    NSLog(@"operation begin --- testSynSerialQueun currentThread = %@", [NSThread currentThread]);
    
    dispatch_queue_t serialQueue = dispatch_queue_create("longchi", DISPATCH_QUEUE_SERIAL);
    dispatch_sync(serialQueue, ^{
        
        for(int i = 0; i < 2 ; i++)
        {
            [NSThread sleepForTimeInterval:2.0f];
            NSLog(@"任务1----- currentThread = [%@]", [NSThread currentThread]);
        }
    });
    
    dispatch_sync(serialQueue, ^{
        
        for(int i = 0; i < 2 ; i++)
        {
            [NSThread sleepForTimeInterval:2.0f];
            NSLog(@"任务2----- currentThread = [%@]", [NSThread currentThread]);
        }
    });
    
    dispatch_sync(serialQueue, ^{
        
        for(int i = 0; i < 2 ; i++)
        {
            [NSThread sleepForTimeInterval:2.0f];
            NSLog(@"任务3----- currentThread = [%@]", [NSThread currentThread]);
        }
    });
    
    
    NSLog(@"operation END ----- currentThread = %@", [NSThread currentThread]);
}

```
执行的结果:
![Alt](/图片/同步串行队列结果.jpg)

可以看出, 任务是在主线程中执按顺序执行, 并且在begin和end之间，主线程需等待任务的完成才会继续执行。


-  异步执行 + 串行队列
```Objective-c

/* 异步串行队列*/
+ (void)testAsyncSerialQueue
{
    NSLog(@"operation begin --- testAsyncSerialQueue currentThread = %@", [NSThread currentThread]);
    
    dispatch_queue_t serialQueue = dispatch_queue_create("longchi", DISPATCH_QUEUE_SERIAL);
    dispatch_async(serialQueue, ^{
        
        for(int i = 0; i < 2 ; i++)
        {
            [NSThread sleepForTimeInterval:1.0f];
            NSLog(@"任务1----- currentThread = [%@]", [NSThread currentThread]);
        }
    });
    
    dispatch_async(serialQueue, ^{
        
        for(int i = 0; i < 2 ; i++)
        {
            [NSThread sleepForTimeInterval:1.0f];
            NSLog(@"任务2----- currentThread = [%@]", [NSThread currentThread]);
        }
    });
    
    dispatch_async(serialQueue, ^{
        
        for(int i = 0; i < 2 ; i++)
        {
            [NSThread sleepForTimeInterval:1.0f];
            NSLog(@"任务3----- currentThread = [%@]", [NSThread currentThread]);
        }
    });
    
    
    NSLog(@"operation END ----- currentThread = %@", [NSThread currentThread]);
}


输出结果：
2018-12-24 22:18:05.296327+0800 DataCollect[54421:12425906] operation begin --- testAsyncSerialQueue currentThread = <NSThread: 0x102003420>{number = 1, name = main}
2018-12-24 22:18:05.296673+0800 DataCollect[54421:12425906] operation END ----- currentThread = <NSThread: 0x102003420>{number = 1, name = main}
2018-12-24 22:18:06.301230+0800 DataCollect[54421:12425941] 任务1----- currentThread = [<NSThread: 0x1004090a0>{number = 2, name = (null)}]
2018-12-24 22:18:07.305726+0800 DataCollect[54421:12425941] 任务1----- currentThread = [<NSThread: 0x1004090a0>{number = 2, name = (null)}]
2018-12-24 22:18:08.308416+0800 DataCollect[54421:12425941] 任务2----- currentThread = [<NSThread: 0x1004090a0>{number = 2, name = (null)}]
2018-12-24 22:18:09.309698+0800 DataCollect[54421:12425941] 任务2----- currentThread = [<NSThread: 0x1004090a0>{number = 2, name = (null)}]
2018-12-24 22:18:10.313609+0800 DataCollect[54421:12425941] 任务3----- currentThread = [<NSThread: 0x1004090a0>{number = 2, name = (null)}]
2018-12-24 22:18:11.315815+0800 DataCollect[54421:12425941] 任务3----- currentThread = [<NSThread: 0x1004090a0>{number = 2, name = (null)}]
Program ended with exit code: 0
```
由结果可看出：任务是按顺序执行的，但是不在begin-end之间， 说明主线程无需等待任务执行完成，可以继续往下执行其他任务。 同时发现，任务不是在主线程执行。说明异步执行创建了新的线程。(异步执行具备创建新线程的能力)

- 同步并发队列

  任务按照顺序执行，都在主线程中执行，说明的同步执行并没有创建新的线程， 任务在begin和end之间执行，说明同步执行任务的特质(需要等待)。

- 异步并发队列

  任务并没有按顺序执行，三个任务都在不同的线程中执行，说明的异步执行创建了新的线程， begin和end打印在前，说明异步执行任务的特质(主线程不需要等待即可继续执行)。

- 同步 + 主线程队列
  发生死锁，因为相互等待引起的，主线程中正在执行当前的方法，方法里又同步等待任务1的执行完毕，而任务1想执行完毕必须先等当前方法执行完毕。

  可以放到其他线程去处理。 需要注意的是，放到主线程队列中的任务最终还是在主线程中执行的。

- 异步 + 主线程队列
发生死锁，因为相互等待引起的，主线程中正在执行当前的方法，方法里又同步等待任务1的执行完毕，而任务1想执行完毕必须先等当前方法执行完毕。