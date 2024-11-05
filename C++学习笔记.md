std::string转换为指针

```C++
std::string text = "hello";
const char* pText = text.c_str();//虽然转为指针 但是你不可以修改指针的内容
```

C++如何获取当前程序运行路径

```C++
	获取当前exe或库的路径
	//typedef unsigned short wchar_t
	wchar_t path[MAX_PATH];	//define MAX_PATH 260 这是C++内部定义的宏 直接使用即可
	GetModuleFileName(NULL, path, MAX_PATH);
	std::wcout << "当前所在路径：" <<  + path  << "!!!!" << std::endl;

	tip:现有一个库A，A里面编写了如上代码，此时独立运行A，则会输出打印A的.dll或.lib的路径（如何独立运行库，如果现在有一个库，写好了代码准备调试，可以不借助另一个程序来调用他，将他自己变为exe
	1.将配置类型改为exe
	2.C/C++预处理器的预定义宏将_WINDOW 替换为 _CONSOLE
	3.链接器所有选项->子系统将 窗口 (/SUBSYSTEM:WINDOWS) 改为 控制台 (/SUBSYSTEM:CONSOLE)
	4.写main函数），若其他控制台程序调用这个库，则会输出打印这个程序的exe路径
```

C++ lambda表达式-匿名函数

```C++
Qt的lambda表达式就是从这里C++的特性引出的
    格式：[](){};
    含义：[]:捕获参数 允许函数体内部使用外部参数 引用捕获或者值捕获 也可以指定捕获某一个、某几个
        	[x] [x,y] [&x,y] [&x,y,&z]
         ():形参 就是普通函数的形参
		 {}:函数体
      
    实例：
        [](){
        std::cout << "hello world！，这是一个由C++lambda表达式形式调用的函数\n";
        
    };
		像上面这样操作就实现了一个在C++里面的lambda表达式的编写
        auto myFun = [](){
        std::cout << "hello world！，这是一个由C++lambda表达式形式调用的函数\n";
        
    };   
		myFun();
		这样就可以调用这个lambda表达式，若此时有返回值，那么这个myFun的类型就是返回值的类型

```

C++中调用线程

```C++
#include<thread>
	1.创建线程 std::thread thread1(func*,arg1);
	这里创建线程的时候需要传入两个参数，第一个参数为某个函数，表示线程开始就调用某个函数开始执行，第二个参数为调用函数的参数
        void myPrintf(std::string msg)
        {
            std::cout << msg << std::endl;
        }

        int main()
        {
            std::thread thread1(myPrint,"hello!!");
        	return 0 ;
        }
	//上述代码在实际运行过程中会报错，因为线程的执行顺序是不一定的，程序执行起来后，执行线程thread1，有可能thread1的函数还没执行完主线程就已经return了，所以导致出错。
	2.join() 特点：阻塞态
        将上述代码修改：
        void myPrintf(std::string msg)
        {
            std::cout << msg << std::endl;
        }

        int main()
        {
            std::thread thread1(myPrint,"hello!!");
            thread1.join();
        	return 0 ;
        }
join()函数用于等待子线程执行完毕，就是说在主线程会等待调用该函数的子线程执行完毕，阻塞；注意，如果线程已经执行结束，再次调用join函数会导致未定义行为，所以一般会在确保线程未结束时调用。
    3.detach() 特点：非阻塞，用法和join一样，不同点是detach不会等待线程执行结束，会将主线程与子线程分离，主线程执行结束后子线程依然在后台执行，实际使用detach很少
    4.joinable():作用：判断当前线程是否可以调用join或者detach函数，返回值bool类型
    	一般在大型项目里面都会判断一下再使用join或者detach，如果在不能调用join和detach的时候调用了，那么会报system_error的错误，有哪些情况不能调用？
    	1.线程已经结束
    	2.如果线程在调用之前已经被销毁，此时再调用就会崩溃
    	3.再同一个线程中调用join，自己不能在自己的线程函数中调用join
    	4.多次调用
    	5.被detach分离之后的线程不能再调用join，因为此时线程已经被分离出主线程，已经独立运行，主线程无法再等待他执行完成
    
    5.线程函数参数为引用类型时，传入一个局部变量则会报错或者直接编译不通过，原因是线程
    
```

