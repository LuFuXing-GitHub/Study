std::string转换为指针

```
std::string text = "hello";
const char* pText = text.c_str();//虽然转为指针 但是你不可以修改指针的内容
```

