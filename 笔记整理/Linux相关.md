vim编辑模式下按空格键不起作用以及按方向键乱码

```c
修改vim编辑器配置文件
	/etc/vim/vimre.tiny //在最后将set compatibal修改为set nocompatibal	这是为了改变乱码
    在这句的下一行添加一句set backspace=1 //设置缩进
```

