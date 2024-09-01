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
QTextEdit：文本编辑器 带格式 支持图片 文字字体、颜色、字号 超链接等 是用场景：处理复杂文字
```

```c++
QToolButton
    相较于QPushButton，QToolButton拥有更多的显示风格、如图标、文字的组合，同时可以搭配QActiopn，与之关联后功能更为强大
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

