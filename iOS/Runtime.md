## 基础 
1.等价写法
```
float (*sendFloatMessage)(id, SEL, id, id, id) = (float (*)(id, SEL, id, id, id))objc_msgSend;
``` 

2. [self class] 与 [super class] 返回的值是子类。

3. MJExtension 实现字典转模型，以及模型转字典。

4. 调用copy的函数需要释放资源free();


链接：https://www.jianshu.com/p/0e6eb2f9ed5d