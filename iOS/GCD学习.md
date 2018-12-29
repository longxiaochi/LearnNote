## iOS  GCD理解

1. GCD 栅栏方法：dispatch_barrier_async
 - 多个异步操作，可以使用dispatch_barrier_async 隔开， dispatch_barrier_async之前的任务执行完成后，才会执行dispatch_barrier_async之后的任务。
 - dispatch_barrier_sync 同步的栅栏执行，一样遵循上面的准则。


 2. GCD 延时执行方法：dispatch_after



