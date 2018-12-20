1. 区分命令行模式与python交互模式。
2. 简单的命令：
   (1)exit() 退出交互模式
   (2)python 进入Python交互模式
   (3)在交互模式中适合调试，执行.py文件需要进入交互模式。
   (4)输入:input,  输出:output
   (5)print中使用,做为空格。 对于双引号和单引号， 可用双引号包含单引号。如果两者都有，则可以使用转义。\
   (6)'''...'''的格式表示多行内容, r''这个符号包含的字符串不进行转义
   (7)python中的布尔：Ture  False, 逻辑词：and or not
   (8)空值是Python里一个特殊的值，用None表示。None不能理解为0，因为0是有意义的，而None是一个特殊的空值
   (9)python中的除运算：/代表着精确运算， //地板除，计算出来的是整数， %求余数操作。
   (10)对于单个字符的编码，Python提供了ord()函数获取字符的整数表示，chr()
   (11)关于编码：ASCII、 Unicode、 UTF-8 编码. ASCII编码只占一个字节， Unicode使用2个字节， 而UTF-8则是不定长的字节编码， 所以一般在网络传输或者本地文件中使用的都是UTF-8编码
   (12)bytes类型的数据用带b前缀的单引号或双引号表示, 区分'ABC'和b'ABC'
   (13)以Unicode标识的字符可以通过encode() 函数来指定编码。 如：'ABC'.encode('ascii'), 
   (14)反过来如果我们想将字节转化为字符，则可以使用decode来转码，如：b'ABC'.decode('ascii'), bytes中只有一小部分无效的字节，可以传入errors='ignore'忽略错误的字节.
   (15)在bytes中，无法显示为ASCII字符的字节，用\x##显示
   (16)len()函数计算的是str的字符数，如果换成bytes，len()函数就计算字节数：
   (17)设置文件的头部： #!/usr/bin/env python3  # -*- coding: utf-8 -*-
   (18)%运算符就是用来格式化字符串的。在字符串内部，%s表示用字符串替换，%d表示用整数替换，有几个%?占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个%?，括号可以省略。 字符串里面的%是一个普通字符怎么办？这个时候就需要转义，用%%来表示一个%：

2.1 list 和 tuple
    (1)使用['kog','xix'] 代表着一个列表。classmates[-1] 取最后的一个元素。
    (2)追加：list.append('xigui') 添加到最后。
    (3)插入到指定的位置:list.insert(2,'hahaha')
    (4)删除：list.pop()从尾部删除， list.pop(0) 从指定地方删除。
    (5)替换成别的元素，可以直接赋值给对应的索引位置:classmates[1] = 'Sarah'
    (6)list 里的数据类型可以不一样。 空list的长度为0，L = []
    (7)元组：tuple, tuple 一旦初始化就不能再修改(说的是指向的地址不能修改，但是地址里面的东西是可以修改的)。tuple 没有append()，insert()这样的方法，其余的和list一样，所以tuple更加安全。 
    (8)定义： t = (1, 2)， 空的tuple， t = (), 但是如果tuple只有一个元素，那么应该这样写t = (2,)
    小结：list 和 tuple都是python内置的有序集合，一个可变，一个不可变。根据需要来选择使用它们。
    
2.2 if/for 语句
    (1)input()返回的数据类型是str，str不能直接和整数比较，必须先把str转换成整数. 字符串转整数：int()
    (2)Python提供一个range()函数，可以生成一个整数序列，再通过list()函数可以转换为list。比如range(5)生成的序列是从0开始小于5的整数：

2.3 dic/set
    (1)dic.get('') dic.remove() dic['']
    (2)在set中，没有重复的key. 通过add和remove 增加和删除值。
    (3)函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量

3.函数:
    (1). def 函数名(参数):
    (2) 导入函数: from testabs import my_abs
    (3) 空函数使用pass, 可以作为占位符。
    (4) 数据类型检查可以用内置函数isinstance(), 而raise TypeError('bad operand type') 用于抛异常。
    (5) Python的函数返回多值其实就是返回一个tuple，但写起来更方便。
    (6) 正常定义的必选参数外，还可以使用默认参数、可变参数和关键字参数
    (7) 定义默认参数要牢记一点：默认参数必须指向不变对象！
    (8) 默认参数、可变参数、关键字参数
        默认参数：传递的是默认的值，可缺
        可变参数：可以传多个，可以是个list和tuple
        关键字参数：可以传dic， 但只是简单的拷贝， 内部的修改不会影响都原有的



4. Mac 在控制台上的配置python3.7
   (1)打开配置文件：open ~/.bash_profile 
   (2)写入python的外部环境： export PATH=${PATH}:/Library/Frameworks/Python.frameworks/Versions/3.7/bin
   (3)重命名python: alias python="/Library/Frameworks/Python.frameworks/Versions/3.7/bin/python3.7"
   (4)关闭文件，source ~/.bash_profile 编译
   (5)在终端输入python

5.常用命令： 
输入:input   输出:output ,print("longchi","小伞")



二、常用函数：
    (1) abs() 绝对值
    (2) max() 最大值
    (3) int() 转int， str()、float()都一样。
    (4)  














