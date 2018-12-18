## const的使用
const表示常量，可以用来修饰不可改变的事物。其用法有很多，如修饰指针、函数等。

- 指针相关
    - const char * p : 表示p是指向char*类型的常量，所以*p是不能变化的，但是这并意味着p所指向的内存是const的，只是对于p来说只const的。
    - char *const p : 表示p是const的，其存储的内存地址不能修改。但是地址所对应的内容却是可以进行修改的。即 p是不能修改，但 *p可以修改。

- 常成员函数 
    - T top() const : 函数使用const修改，表示是常成员函数，常成员函数特点：(1)在常成员函数中，不能修改成员变量的值. (2)常成员函数中只能调用常成员函数，不能调用普通的成员函数。(3)const关键字可以重载函数名相同但是未加const关键字的函数.

    - 例子:
         ```c++
            // An highlighted block
            class TestConst
            {
            public:
                void shakHand();
                void shakHand() const;
                
                void print() const
                {
                    cout << "print" << endl;
                };
                
                void print2()
                {
                    cout << "print2" << endl;
                };
                
            private:
                string element;
            };


            void TestConst::shakHand()
            {
                //普通成员函数也可以调用const修改的函数
                print();
                
                print2();
                
                element = "123";
            };

            void TestConst::shakHand() const
            {
                //常成员函数中可以调用的常成员函数
                print();
                
                //报错，不能调用普通的成员函数
            //    print2();
                
                //报错，常成员函数中不能修改类的成员变量
            //    element = "123";
            };


            int main()
            {
                TestConst testConst;
                testConst.shakHand();
                
                const TestConst testConst2;
                //将调用const修改是shakHand
                testConst2.shakHand();

                return 0;
            }
        ```