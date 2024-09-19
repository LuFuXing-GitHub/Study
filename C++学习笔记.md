std::string转换为指针

```C++
std::string text = "hello";
const char* pText = text.c_str();//虽然转为指针 但是你不可以修改指针的内容
```

C++如何获取当前程序运行路径

```C++
	获取当前exe或库的路径
	//typedef unsigned short wchar_t
	wchar_t path[MAX_PATH];	//define MAX_PATH 260
	GetModuleFileName(NULL, path, MAX_PATH);
	std::wcout << "当前所在路径：" <<  + path  << "!!!!" << std::endl;

	tip:若在某个库里调用了这个函数会获取当前库的路径，如果这个库被某个程序调用了也会获得该程序的exe路径
```

