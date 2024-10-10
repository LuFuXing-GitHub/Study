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
    含义：[]:捕获参数
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

