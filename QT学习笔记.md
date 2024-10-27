```cpp
容器类：
    Qt有很多基于模板的容器类
顺序容器类：
    QList<T>	QLinkedlist<T>	QVector<T>	QStack<T>	QQueue<T>
    QList<T>:底层是数组;
    QLinkedlist<T>:底层是链表;
	QVector<T>:底层是数组;
	QStack<T>:底层是数组，即QVector<T>;
	QQueue<T>:底层是数组，即QList<T>;
关联容器类：
    QMap<T>	QMultimap<T>	QHash<T>	QMultihash<T>	QSet<T>
    QMap<T>是基于红黑树的，是一对一的，如果新插入的数据已有键，那么会更新为新插入的值；但是也可以是一对多，一个键对应的值为一个数组时即可。
    概括：
    QMap使用insert插入数据，如果已有键则会更新数据，如果有多个相同的键那么会更新最新一个插入的键的值；
    QMultimap使用insert插入数据，相当于QMap的insertMulti；QMultimap自己也有insertMulti，实际上没啥区别。但是insertMulti会直接往最后插入数据，而insert在已有键的情况下才会直接往最后插入。QMultimap的replace相当于QMap的insert。
    
    
    Hash table哈希表介绍：
    什么是哈希表？
    若现在有一段数据：0 3 5 6 8 9 10……假设有一百个
    难么现在定义一个长度为一百的数组，使用!!!下表来记录元素是否出现，出现了多少次!!!
    int hashTable[100];
	例如：3出现5次，那么hashTable[3] == 5;
		 8出现3次，那么hashTable[8] == 3;
    这样整理完之后，便可通过该数组的下标来直接查找数据
        例如：hashTable[3] == 0;//表示没有存储3这个数据
			 hashTable[4] == 2;//表示存储了两个4
	所以hash表的查询速度非常快，理想状态为O(1);
	哈希算法的简单例子：
        #define LEN 100
        int main()
        {

            int data[10] = { 1,6,54,7,5,2,41,2,63,63 };

            int hashTable[LEN] = { 0 };

            for (int i = 0; i < 10; i++)
            {
                hashTable[data[i]]++;

            }

            for (int i = 0; i < LEN; i++)
            {
                if (hashTable[i] > 0)
                {
                    printf("%d有%d个\n",i,hashTable[i]);
                }

            }

            //查询某个值key
            int key = 9;

            if (hashTable[key] > 0)
                printf("找到该数据，且该数据存储个数为%d\n", hashTable[key]);
            else
                printf("没有该数据\n");

            return 0;
        }


	哈希函数：为什么需要哈希函数？
        上个例子中，所存的数据在100以内，可以实现，但如果存的数据是很大的该怎么办？
        若为int的范围2^32大小，我们可以通过将所有数据对哈希表大小取余来进行整理存储。
        若数据类型为字符串，则可以通过算他的ASCII码存储。
        !!!解决不同场景的数据，将其整理映射为整数作为哈希表的下表进行存储，称为哈希函数!!!。
     但是会出现比如：523%100 = 23；423 %100 = 23；同时23也是23，那么就导致查询到的数据不知道是哪个？导致冲突，所以还需要解决冲突。
        解决冲突为设计哈希算法的重要步骤
	哈希冲突：!!!解决出现相同索引存储多个键的情况!!!
        链地址法：虽然存储了多个键，但是键值对之间采用链表进行存储
        线性探测、二次探测：若索引位置已存储键，那么往数组下一个有效位置存储（例如数组末尾存下一个键）
        
重点：
	QMap的键必须提供“<”运算符，QHash的键必须提供“==”运算符和一个名称为qHash()的全局散列函数
    因为QMap的键存储是有顺序的，所以当我们使用自定义数据类型作为键插入QMap时，必须重载运算符“<”。这是因为QMap内部使用类似于二叉搜索树的数据结构来保持有序性，需要能够比较键的大小来进行插入、查找等操作。
        
    因为QHash是通过哈希表进行存储的，所以必须重载运算符“==”才能对查找时的键进行比较其相等性。

    要想将自定义数据类型作为QMap的键，则需要重载运算符“<”，因为QMap是由顺序的，需要比较大小
        例如：
         bool operator<(const Mydata& x)const
        {
            return age < x.age;
        }
	要想将自定义数据类型作为QHash的键，则需要重载“==”运算符，因为他需要对比对应索引存储的值是否相等，且需要在全局实现内联函数qHash()，其目的是为了告诉编译器你将如何把自定义数据类型映射为整数存储到哈希表中。
        例如：
        inline uint qHash(const Mydata& key,uint seek = 0)
        {
            return qHash(key.getAge(),seek) ^ qHash(key.getName(),seek);
        }
```

```cpp
Qt的隐式共享：
    隐式共享是对象的一种管理方法，当一个对象被隐式共享，只是传递对象的指针给使用者，而不实际复制其值，只有当使用者修改数据时，才实际复制值给使用者
    QString str1 = "heelo";
	QString str2 = str1;	//这里就发生了隐式共享
	str2 += "wolrd";	//这里才实际发生了复制操作
节省内存，减少不必要的内存消耗，若复制的数据类型很复杂但又不需要修改其值；提高性能，特别是在多线程情况下减少不必要的内存开销对整体提升很大；Qt自带数据类型绝大多数都是隐式共享，用户自定义数据类型则需要通过继承于QSharedData即可；

```

```c++
字符串的输入输出：
    QString存储字符串采用Unicode编码，每一个字符是一个16位的QChar，并非8位的Char，一个汉字算一个字符
```

```C++
QCombobox
下拉框，为用户提供一个下拉框
使用addItem或addItems添加列表项；
例如：
	com->addItem("北京")；
	com->addItem("上海")；
	com->addItem("深圳")；
	这个函数还有第二个参数，可以传入任何数据类型，此参数为第一个参数的关联数据，但是用户只看得到第一个参数；
	使用currentData()或者itemData(int&)获取当前数据项的关联数据
```

```c++
QPlanTextEdit和QTextEdit
QPlanTextEdit:纯文本编辑器 不带格式 不能插入图片 超链接等 适用场景：处理大量文字 如服务器日志输出
QTextEdit：文本编辑器 带格式 支持图片 文字字体、颜色、字号 超链接等 使用场景：处理复杂文字
```

```c++
QToolButton
    相较于QPushButton，QToolButton拥有更多的显示风格、如图标、文字的组合，同时可以搭配QAction，与之关联后功能更为强大
	适用场景：菜单栏 工具栏 QToolButton关联

QAction-项：非常有用的一个类
    在QMainWindow中，可以将其作为一个菜单的一项，菜单栏->添加菜单，可以在ui设计器里下面的Action Editor里手动把设置好的项拖到菜单中，只需编写该项的triggered信号以及该项的文本、图标等
    可以作为工具栏的一项，操作方法同上
    可以将其与QToolButton关联起来，点击QToolButton按钮就是触发QAction项的triggered信号
```

```C
	QTreeWidget-树组件
        
        
        
```

```c++
小结：QListWidget QTreeWidget QTableWidget都可以设置项的关联数据setData(Qt::UserRole,QVariant());
最后通过data获取该关联数据
    QListWidgetItem* wisItem = new QListWidgetItem(wid,1001);
	像如此创建项的时候给第二个type参数，后续可通过此参数判断该项的类型（ QTreeWidget QTableWidget）同理
        
```

```
Model/View结构
数据源->模型（数据模型）->视图（视图组件）
常用于数据库应用程序中，直接操作界面上的数据实际上是修改了界面组件所关联的数据库内的数据。 这种结构叫做Model/View结构
代理：用户可以定制数据的显示和编辑方式
```

```c++
QFileSystemModel：访问本机文件系统的数据模型，与视图QTreeView组合使用，可以用目录树的形式显示本机上的文件系统，使用其接口函数可实现创建目录、删除目录、重命名目录；获取文件名称、目录名称、文件大小、文件详细信息等操作；
    QFileSystemModel* fmodel = new QFileSystemModel;
	fmodel->setRootpath(QDI::currentPath());
    
QDirModel:与QFileSystemModel作用一样，区别如下：
    QFileSystemModel：采用单独的线程获取目录文件结构
    QDirModel：不使用单独的线程
    使用单独的线程就不会阻塞主线程，一般推荐QFileSystemModel
    
    
QStandardItemModel：标准项模型 配合QItemSelectionModel（项选择模型）使用
    通常与QTableView配合形成Model/View结构
    //代码解析
    theModel = new QStandardItemModel(2,FixedColumnCount,this);//创建模型时给出行和列
    theSelection = new QItemSelectionModel(theModel);//创建项选择模型时传入项模型

    connect(theSelection,&QItemSelectionModel::currentChanged,this,&MainWindow::on_currentChanged);//连接项选择模型的信号与槽函数（槽函数是自己写的） 该信号带有两个参数 一个为当前项 一个为前一项

    ui->tableView->setModel(theModel);//设置视图组件的模型
    ui->tableView->setSelectionModel(theSelection);//设置视图组件的项选择模型
    ui->tableView->setSelectionMode(QAbstractItemView::ExtendedSelection);//设置视图组件的项选择方式 可扩展
    ui->tableView->setSelectionBehavior(QAbstractItemView::SelectItems);//设置视图组件的项选择行为 选择多项
    

	槽函数：
    if(current.isValid())
    {
        LabCurFile->setText(QString::asprintf("当前单元格行：%d 列：%d",current.row(),current.column()));
        QStandardItem* aItem = theModel->itemFromIndex(current);
        LabCellText->setText("当前单元格内容：" + aItem->text());
        QFont font = aItem->font();
        ui->actBold->setChecked(font.bold());
    }
    if(prevois.isValid())
    {
        QStandardItem* aItem = theModel->itemFromIndex(prevois);
        if(aItem)
        {
            LabPreText->setText("前一项单元格内容：" + aItem->text());
        }

    }

打开一个文件：
     QString curPath = QCoreApplication::applicationDirPath();
    QString fileName = QFileDialog::getOpenFileName(this,"打开文件",curPath + "../../../","文本文件(*.txt *.h *.cpp)");

    if(fileName.isEmpty())
        return;

    QStringList fileContent;
    QFile file(fileName);

    if(file.open(QIODevice::ReadOnly | QIODevice::Text))
    {
        QTextStream stream(&file);
        stream.setCodec(QTextCodec::codecForName("UTF-8"));//这里是解决乱码，若知道源文件的编码格式为UTF-8 那么直接设置读取文件流的编码格式为？UTF-8
        ui->plainTextEdit->clear();
        while (!stream.atEnd())
        {
            QString str = stream.readLine();//这里解释循环为什么能实现读取文件内容的功能 因为readLine函数读取了一行内容后会自动将文件流指针往下指一行
            ui->plainTextEdit->appendPlainText(str);
            fileContent.append(str);

        }

        file.close();
        LabCurFile->setText("当前文件：" + fileName);
    }
    


！！！！！模型才有setData函数，项没有，项的数据在构造函数时直接给！！！！！
```



**自定义代理**：

```c++

自定义代理：
    总的基类：QAbstractItemDelegate
    派生类：QItemDeledate	QStyledItemDelegate
    自定义代理需要从派生类中选择一个作为基类，通常选择QStyledItemDelegate，因为他支持是用样式表绘制组件，说变了就是支持更复杂、更花哨的组件的显示
    在当前项目中新增一个C++类并选择其基类为QStyledItemDelegate：
    项目右键->AddNew->C++的C++Class，创建完成后添加#include <QStyledItemDelegate>头文件，然后公有继承QStyledItemDelegate类
    重写4个主要函数
    createEditor();//创建编辑组件
	setEditorData();//从模型内部获取数据设置当前组件的数据
	setModelData();//从当前组件获取数据设置到模型内
	updateEditorGeometry();//设置组件的大小
以SPinBOx为例：
    //形参解析 这里的parent是界面视图组件 option视图项的样式选项，包含有关该项的显示、尺寸和位置等样式信息。用于创建与项样式一致的编辑器控件 就是原来视图的项的信息（就是原来那个视图组件的某个单元格的信息 比如大小 或者当前被选中没有）  index模型索引
QWidget *QWIntSpinDelegate::createEditor(QWidget *parent, const QStyleOptionViewItem &option, const QModelIndex &index) const
{
    QSpinBox* editor = new QSpinBox(parent);
    editor->setFrame(false);
    editor->setValue(66);
    return  editor;
}
	//editor编辑组件 index模型索引
void QWIntSpinDelegate::setEditorData(QWidget *editor, const QModelIndex &index) const
{
    int val = index.model()->data(index,Qt::EditRole).toUInt();
    QSpinBox* spin = static_cast<QSpinBox*>(editor);
    spin->setValue(val);
}
	//editor编辑组件 model模型 index模型索引
void QWIntSpinDelegate::setModelData(QWidget *editor, QAbstractItemModel *model, const QModelIndex &index) const
{
    int val = static_cast<QSpinBox*>(editor)->value();
    //QSpinBox* spin = static_cast<QSpinBox*>(editor);
    //spin->interpretText();
    //int val = spin->value();
    model->setData(index,val,Qt::EditRole);

}
	//editor编辑组件 option包含了一些
void QWIntSpinDelegate::updateEditorGeometry(QWidget *editor, const QStyleOptionViewItem &option, const QModelIndex &index) const
{
    editor->setGeometry(option.rect);
}
```

***对话框***

```c++
QFileDialog文件对话框：
    获取打开文件：
    QString fileName = QFileDialog::getOpenFileName(this,"选择一个文件","../","文本文件(*.txt *.h);;图片(*.jpg)");
    if(fileName.isEmpty())
    {
        ui->plainTextEdit->appendPlainText("当前未选择文件");
    }
    {
        ui->plainTextEdit->appendPlainText("当前用户选择的文件：" + fileName);
    }

	获取打开的多个文件：
    QStringList fileNames = QFileDialog::getOpenFileNames(this,"选择多个文件","../","所有文件(*.*)");
    if(fileNames.isEmpty())
    {
        ui->plainTextEdit->appendPlainText("当前未选择文件");
    }
    else
    {
        for(auto name : fileNames)
        {
            ui->plainTextEdit->appendPlainText("当前用户选择的文件：" + name);
        }

    }
	不可使用于同一按钮的槽函数中，因为调用了两次函数
    选择已有目录对话框：
    QString dir = QFileDialog::getExistingDirectory(this,"选择一个目录","../",QFileDialog::ShowDirsOnly);
    if(dir.isEmpty())
    {
        ui->plainTextEdit->appendPlainText("用户未选择目录");
    }
    else
    {
        ui->plainTextEdit->appendPlainText("选择目录为：" + dir);
    }
	选择保存文件名
     QString fileName = QFileDialog::getSaveFileName(this,"保存文件","../","所有文件(*.*)");
    if(fileName.isEmpty())
    {
        ui->plainTextEdit->appendPlainText("用户未选择目录");
    }
    else
    {
        ui->plainTextEdit->appendPlainText("选择目录为：" + fileName);
    }
	如果选择了一个已经存在的文件名，那么它会提示是否覆盖，并返回这个文件名（但是实际上并没有进行了覆盖操作，需手动代码完成）
    如果手动输入了文件名，那么会返回这个文件名（但是实际上并没有创建这个文件）
       
        
     
颜色对话框QColorDialog：
    QPalette pal = ui->plainTextEdit->palette();//获取当前调色板
    QColor iniColor = pal.color(QPalette::Text);//通过调色板获取当前颜色
    QColor color = QColorDialog::getColor(iniColor,this,"选择颜色");//将当前颜色作为参数调用该函数 返回用户选择的颜色
    if(color.isValid())
    {
        pal.setColor(QPalette::Text,color);
        ui->plainTextEdit->setPalette(pal);
    }

字体对话框QFontDialog
    QFont font = ui->plainTextEdit->font();
    bool ok;
    QFont setFont = QFontDialog::getFont(&ok,font);
    if(ok)
    {
        ui->plainTextEdit->setFont(setFont);
    }
```

**如何使用Qt creator创建和使用Qt C++库**

```C++
创建->选择library，到Details这里选择要创建的库类型，动态库、静态库、qt插件
    动静态库这里不做过多解释，vs阶段已经学过
    qt插件-Qt Plugin也是一种动态库，可以将其理解为Qt特有的一种动态库，与普通动态库在实现上略有差异
    
   	静态库(Linked Library)创建
    	直接选择静态库模板，进去之后直接在头文件里面声明类、函数、变量等
    动态库(Shared Linked Library)创建
    	创建成功后，Qt会自动创建三个文件，xx.h xx.cpp xx_qt_global.h
    	在xx_qt_global.h头文件里面Qt会自动创建动态库编译宏，只需在xx.h文件里编写对外的接口函数、类、变量即可
    
    
    如何调用库
    	在即将调用库的程序里，修改.pro文件：
    	LIBS += -LE:\Projects\Qt_creator\library\build-lib_for_qt-Desktop_Qt_5_14_2_MinGW_32_bit-Debug\debug -lliblib_for_qt	//大写的L表示库所在的路径 小写的l表示库的名字，且不需要加扩展名
    	INCLUDEPATH += //头文件的路径
	qmake:右键->执行qmake
        会生成makefile文件，他会去读当前项目的.pro配置文件，比如需要依赖哪些库，就会生成对应的makefile文件，在执行构建的时候会执行makefile文件

        总结就是：qmake生成的makefile文件会编译当前项目的文件，就跟正常的makefile文件一样，但是如果该项目有依赖外部库，makefile并不会编译其他项目来生成库
```

Qt如何开发Android手机应用

```C++
默认使用Qt creator开发
    打开工具->选项->设备，右边选择Android，下载三个东西
    JDK(JAva相关):
	SDK:网站进不去，网盘下载别人的资源
	NDK:网站进不去，网盘下载别人的资源
        
    下载好后直接安装JDK，有两个安装的，不知道啥原因，但是安装好后Qt creator会自动识别到JDK的路径，SDK和NDK一样，点击exe选择路径，之后会在路径下生成安装包，解压该安装包
    配置好三个路径
        
    注意：如果JAVA安装的时候未选择默认路径，那么安装后需要添加环境变量，而且是系统的环境变量！！！！！
        JAVA_HOME=D:\JDK//这里的路径是SDK最开始安装的那个路径
        CLASSPATH=.,%JAVA_HOME%\lib,%JAVA_HOME%\lib\tools.jar
            最后在PATH里面加入这个变量：
            Path=%JAVA_HOME%\bin，记得要把它移到最上面去
        如若未默认安装路径，又不进行上述添加环境变量的步骤，那么编译到后一步就会一直报错
            
   	创建新的工程，编译器选择Android
        
    在首次编译项目的时候Qt会下载一个gradle构建工具，这个工具下载的奇慢无比，可以去镜像下载对应的版本，下载完成之后，将这个压缩包放到默认路径下，Windows下是：
        C:\Users\用户名\.gradle\wrapper\dists\gradle-5.5.1-bin\cfsov38hb3r1zj4ic9bbjcc7n
        我的是
        C:\Users\卢福兴\.gradle\wrapper\dists\gradle-5.5.1-bin\cfsov38hb3r1zj4ic9bbjcc7n
            注意是将整个压缩包放到这里，编译的时候Qt会自己解压
        
       
```

背景图设置不成功原因：

```C++
在designer中设置背景图，且看见了效果，但是运行起来的时候却没有背景图，这是因为Qt不让设置最高等级为QWidget或者QMainWindow的设置背景图，因为其他所有的控件都会继承父类，都会有这个背景图效果，解决方法：
    1.设置完之后调用一个函数setAttribute(Qt::WA_StyledBackground);这个属性什么意思呢？我们查阅帮助文档得知，意思就是告诉窗口，应该使用设置的样式表进行渲染控件背景
    2.在构造函数中使用代码设置样式表setStyleSheet("background-image: url(:/pic/log.jpg);");
```

Qt如何发布程序

```C++
用mingw编译：
    将可执行程序直接放到某个文件夹下，添加对应编译器的bin目录到环境变量Path里面，比如用的是mingw64，就在Path里添加D:\Qt\5.14.2\mingw73_64\bin，然后进入cmd，输入命令：windeployqt xx.exe(就是你的可执行程序名称)，之后QT的这个windeployqt就会把当前程序所需要的动态库以及一些文件复制到当前路径下，然后删除环境变量里这个值，双击打开程序，还是会提示缺库，缺的库在对应的编译器目录下，比如这里就从D:\Qt\5.14.2\mingw73_64\bin下寻找丢失的库，一个一个复制过来进行测试。
    打开程序测试没问题之后，就可以进行下一步制作安装程序。
```

TCP通信

```C++
由于TCP是面向连接、可靠的通信方式，所有是C/S结构
服务器端：
    头文件：
    #include<QTcpSocket>	//服务器端与客户端通信套接字
    #include<QTcpServer>	//TCP服务器类
    #include <QHostAddress>	//IP类
    
    流程如下：
    	初始化服务器端，设置监听客户端的ip和端口，连接信号与槽；
    	1.QTcpServer *server = new QTcpServer(this);
    	2.if(server->listen(QHostAddress::Any,8888));
			//Any：监听客户端IP地址 这里的QHostAddress::Any表示监听任意IP地址的客户端，也可以直接填IP地址，比如192.168.2.6，表示只监听IP为此的客户端
			//8888：端口号，设置服务器的端口号，所有连接到此的客户端都需要这个端口号来进行连接，如果填0则表示端口号由服务器自动分配，可以通过函数打印查看分配到的端口号，但是实际使用不建议这样做，因为这会导致客户端无法连接
		3.QTcpServer::newConnection信号
            //该信号表示有客户端连接的信号，可在槽函数里做相应逻辑处理
            void ChatWithMe_Server::slots_newConnection()
            {
                QTcpSocket* nSocket = m_server->nextPendingConnection();
                m_listSocket.push_back(nSocket);
                connect(nSocket, &QTcpSocket::readyRead, this, &ChatWithMe_Server::slots_hasDataReceived);
                qDebug() << u8"IP地址为：" << nSocket->peerAddress();
                qDebug() << u8"端口号为：" << nSocket->peerPort();
            
                QHostInfo info = QHostInfo::fromName(nSocket->peerAddress().toString());
                qDebug() << u8"主机名称为：" << info.hostName();
            }
		4.QTcpServer::nextPendingConnection()函数
            //使用该函数获取当前连接成功的客户端套接字，后续通信则全通过该套接字进行
        5.QTcpSocket::readyRead信号
            //该信号表示当前套接字中有数据被发送过来，可在槽函数里进行消息的接收、显示到ui上等操作
        6.QTcpSocket::disconnected信号
            //该信号表示当前客户端断开连接，可适当打印或者弹框提示
            
 客户端：
	头文件：
    	#include<QTcpSocket>	//只需一个其实就够了 通信套接字
            
    流程如下：
            初始化套接字，连接服务器，连接信号与槽
            1.QTcpSocket* socket = new QTcpSocket(this);
			2.QTcpSocket::connectToHost();
            //这里有两种连接到服务器的方式
			方式一：使用IP地址连接，直接填入服务器端的IP地址和端口号
            	socket->connectToHost(192.168.6.6,5566);
			方式二：使用服务器的主机名连接
                socket->connectToHost("DESKTOP-PN0MM4N",5566);
            方式一连接可能会导致服务器端使用dsn域名解析的时候解析不到客户端的主机名
            3.QTcpSocket::connected信号
                //该信号表示当前客户端连接服务器成功，可在槽函数中做调试打印或弹框提示
            4.QTcpSocket::readyRead信号
            //该信号表示当前套接字中有数据被发送过来，可在槽函数里进行消息的接收、显示到ui上等操作
        	5.QTcpSocket::disconnected信号
            //该信号表示当前客户端断开连接，可适当打印或者弹框提示
            
           QTcpSocket::write()//通过套接字发送数据
           QString类型的需要进行转换，
                QString data = u8"你好";
				socket->write(data.toUtf8());
			接收显示的时候：
				QByteArry msg = socket->readAll();
				ui.lineEdit->setText(QString::fromUtf8(msg));
			实际上可以看出来，由于TCP套接字通信使用的是QByteArry字节数组，而平时打字用的是QString所以需要转换，QString转到
QbyteArray用的是toUtf8()，QByteArray转到QString用的是QString::fromUtf8();



```

