# signal 的简单使用
C++ 提供了一套信息捕获机制，用户捕获程序运行过程中的各种信号。

1. 首先进行注册: signal(SIGINT, signalHandler);

2. 自动制造信号：raise(SIGINT);