# QT

## 1.QT的概述

**QT的发展史：**

​	1991年，奇趣科技开发

**QT的优点**

​	1.跨平台

​	2.接口简单，容易上手

​	3.一定程度上简化了内存的回收（new之后，不需要回收）

**成功案例**

​	1.linux桌面开发KDE

​	2.wps开发

​	3.网络电话

​	4.谷歌地图

​	5.VLC多媒体播放器

​	6.虚拟机软件

​	

## 2.QT的项目创建

​    1.选择QT模板

​	2.选择路径，不能包含中文

​	3.类信息

> ​		基类
>
> ​					QWidget
>
> ​					QmainWindow
>
> ​					QDialog



​			

## 3.QT的基础教程

**快捷键**

运行 Ctrl+r

编译 Ctrl+b

整行移动 Ctrl+shift+方向键

自动对齐 Ctrl+i

同名h和cpp的移动 F4

查找关键字 Ctrl+f

## 4.QPushButton

按钮

包含头文件<QPushButton>

设置按钮时，要开辟一个新的空间

将按钮显示在当前行：setParent("this"); //传入的是父窗口的指针

设置按钮文本：setText

设置按钮大小：resize 	 固定大小 setFixedSize



```
Widget::Widget(QWidget *parent)   : QWidget(parent)
{
    QPushButton *ibt = new QPushButton("你好！");
    //显示在当前窗口
    ibt->setParent(this); //让按钮显示在主窗口上
    ibt->setText("你好");	 //按钮上的标注

    //创建第二个按钮
    QPushButton *btn = new QPushButton("哈喽！",this); //创建按钮的常用方式
    btn->move(200,0);	//移动按钮位置 （x,y）
    btn->resize(100,200);	//设置按钮大小
    //this->resize(600,400);	//设置窗口大小（可调节）
    this->setFixedSize(600,400);	//固定窗口大小
    this->setWindowTitle("第一个窗口");	//设置窗口标题

}
```

## 5.对象树

指的是有一个基类父亲，其子节点都是由父类衍生而来的

当释放时，由下而上释放，但是析构是由上而下调用的，因为调用完析构之后，首先不会释放对象，而是查看是否还有子类对象，重复上述操作

## 6.坐标系

右为x正，下为y正

## 7.信号槽

```
connect(btn,&QPushButton::clicked,this,&Widget::close);
```

> ​				对象				信号						接受       		操作			



#### 	**自定义一个信号槽**

1.创建一个学生类一个老师类    定义一个学生和一个老师

2.为其分配空间   然后在各自的类内声明函数，去对应的文件实现（输出信号的函数只需要声明，不需要实现）

3.关键：emit 确保信号会发出？

```
Widget::Widget(QWidget *parent)
    : QWidget(parent)
{
    this->teacher = new Teacher2;
    this->student = new Student2;
    connect(teacher,&Teacher2::hungry,student,&Student2::Treat);
    this->ClassIsOver();	 //关键代码
}
void Widget::ClassIsOver()   //关键代码
{
    emit teacher->hungry();
}
```

难点：有参的传入时，将类型的强转，避免二义性，const_cast     static

​			有参：在编译时，编译器会自动转换成字符串型，所以打印出的数字带有双引号

​					1.将QString转化为char *		qDebug()<<"  "<<what.toUtf8().data();	

​					2.qDebug().noquote<<"  "<<what;	

**扩展**

1.一个信号可以对应多个槽

2.一个槽可以对应多个信号

3.信号可以连接信号

4.信号之间可以断开练习，和连接的方法一样

5.信号和槽的关系		信号和槽的参数类型要相同

​									  信号的参数个数要大于槽的参数类型

#### **QT4:**

```
connect("信号发出者",SIGNAL(对应函数[hungry()])，
		"信号接受者",SLOT(对应函数)[treat()]);
```

但是当调用函数时，名字输错，不会报错，编译时出现问题

#### lambda表达式

```
[capture](parameters) opt->retType
    {

    }
```

capture  捕获外部局部变量的列表，值传递，直接填变量名

​	使用=值传递捕获所有局部变量

​	使用&应用传递捕获所有局部变量

如果是值传递的话，捕获进来的默认是const类型的，需要修改的话要是有mutable选项

返回值的类型可以忽略（忽略不代表为空），编译器器自动计算返回值

```
推荐使用:[=](){}的形式
```

如果将=换为&时，当局部变量释放时，指针可能变成野指针，使得传入的数据错误

```
 QPushButton *btn = new QPushButton("按钮",this);
    btn->show();
    connect(btn,&QPushButton::clicked,[=]()
    {
        qDebug()<<"hello world";
    });
```

## 8.制作一个记事本

QMenubar用来读取或生成一个菜单栏

**菜单栏**

```
 this->resize(600,400);
    //菜单栏，获取当前窗口的菜单栏，若没有则自动创建一个
    QMenuBar *mb = this->menuBar();
    //添加菜单
    QMenu *menuFile = mb->addMenu("文件");
    QMenu *menuEdit = mb->addMenu("编辑");
    //往菜单中添加菜单项
    QAction *actionNew =  menuFile->addAction("新建");
    QAction *actionOpon =  menuFile->addAction("打开");

    //添加分割符
    menuFile->addSeparator();

    //添加二级菜单
    QMenu *menuRecent = menuFile->addMenu("最近打开的文件");
    menuRecent->addAction("1.txt");
```

**工具栏**

```
    //工具栏，可以多个
    QToolBar *toolBar = this->addToolBar("");

    //工具栏添加工具
    toolBar->addAction(actionNew);
    toolBar->addAction(actionOpen);
    QAction *MyAction = menuFile->addAction("自制");
    toolBar->addAction(MyAction);

    //设置默认停靠右边
    this->addToolBar(Qt::LeftToolBarArea,toolBar);

    //设置只允许停在左边或右边
    toolBar->setAllowedAreas(Qt::LeftToolBarArea|Qt::RightToolBarArea);

    //设置不允许悬浮
    toolBar->setFloatable(false);

    //设置不允许移动
    toolBar->setMovable(false);
```

**状态栏**

```
头文件: 	#include<QStatusBar>    #include<Label>
生成状态栏: QStatusBar *Sb = this->StatusBar;
生成标签: 	QLabel *LabelLeft = new QLabel("左侧信息",this);
		   Sb->addWidget(LabelLeft);
           QLabel *LabelRight = new QLAbel("右侧信息",this);
           Sb->addPermanentWidget(LabelRight);
```

**停靠部件和中心部分**

```
#include<QDockWidget>	#include<QText>
QDockWidget *qDockWidget = new QDockWidget("停靠部件",this);
this->addPermanentWidget(Qt::BottomDockWidgetArea,qDockWidget);
//注 添加停靠部件要添加上位置

QTextEdit *text = new QTextEdit(this);
this->addCentralWidger(text);
```

## 9.**使用ui文件**



## 10.**使用资源文件**

​	用于读取文件中的图片或者音乐

```
ui->actionNew->setIcon
 			(QIcon(":/new/prefix1/wallhaven-e7g7zk.jpg"));
ui>actionOpen>setIcon(QIcon
("C:\\Users\\Lenovo\\Pictures\\Camera Roll\\wallhaven-e7g7zk.jpg"));
```

## 11.模态对话框和非模态对话框的创建

模态：

```
connect(ui->actionModal,&QAction::triggered,[=](){
        //模态对话框
        QDialog log (this);
        log.exec();
        qDebug()<<"hello dialog";
    });
```

非模态:

```
connect(ui->actionNonModal,&QAction::triggered,[=](){
        //非模态对话框
        QDialog *log = new QDialog(this);
        log->setAttribute(Qt::WA_DeleteOnClose);
       log->show();
        qDebug()<<"hello Nondialog";
    });
```

## 12.QMessageBox的使用

```
void MainWindow::on_actionCritial_triggered()
{
    QMessageBox::critical(this,"错误","critial");
}

void MainWindow::on_actionWorning_triggered()
{
    QMessageBox::warning(this,"警告","Warning");
}

void MainWindow::on_actionInfo_triggered()
{
    QMessageBox::information(this,"信息","information");
}

void MainWindow::on_actionQuestion_triggered()
{
   if(QMessageBox::Ok==QMessageBox::question(this,"你好","请问你有对象吗？",QMessageBox::Ok|QMessageBox::Cancel))
   {
       qDebug()<<"选择了Ok";
   }
   else
   {
       qDebug()<<"选择了Cancel";
   }
}
```

## 13.文件对话框

```
void MainWindow::on_actionFileDialog_triggered()
{
    QString FIleName = QFileDialog::getOpenFileName(this,"打开一个文件",
                                  "C:\\Users\\Lenovo\\Pictures\\Camera Roll",
                                  "PNG (*.png);;JPG (*.jpg);;ALL (*.*)");
    qDebug()<<FIleName;
}
```

## 14.布局

**分类：静态布局和动态布局**

静态布局是指位置和大小不会随着外部窗口的改变而改变

动态布局是指位置和大小会随着外部窗口的改变而改变

**常用的动态布局：**

​		水平，垂直，栅格，表单布局

​		推荐使用widget的自带布局

使用弹簧来调整布局的位置，居中

栅格布局可以讲空间分为几行几类

要调整固定窗口大小，就是将最大值和最小值设为相同即可

## 15.按钮组

## 16.ListWidget

​	相当于是一行一行的文字，类似古诗的形式

​	向ListWidget中添加文字	

```
QListWidgetItem *item = new QListWidgetItem("窗前明月光")；
item->setTextAlignment(Qt::AlignHCenter);
ui->ListWidget->addItem(item);

//QStirng-> std::string 		QList->std::List
//QStringList-> std::List(std::String);

QStringList list;
list<<"床前明月光"<<"疑是地上霜"<<"举头望明月"<<"低头思故乡";
ui->listWidget->addItems(list);
```

## 17.TreeWidget

```
第一层：ui->setHearderLabels(QStingList()<<""<<"");
第二层：QTreeWidgerItem *Item = QTreeWidgetItem(QStringList()<<""<<"");
第三层：Item->addChild(new QStingWidgetItem(QStingList()<<""<<""));
```



```
   ui->treeWidget->setHeaderLabels(QStringList()<<"英雄"<<"简介");
    QTreeWidgetItem *liliangItem = new QTreeWidgetItem(QStringList()<<"力量");
    ui->treeWidget->addTopLevelItem(liliangItem);
    liliangItem->addChild(new QTreeWidgetItem(QStringList()<<"刚猪"<<"能力"));
    QTreeWidgetItem *minjieItem = new QTreeWidgetItem(QStringList()<<"敏捷");
    ui->treeWidget->addTopLevelItem(minjieItem);
    minjieItem->addChild(new QTreeWidgetItem(QStringList()<<"悟空"<<"七十二变"));
```

## 18.TableWidget

> 1.设置行数，列数  setRowCount  setColumnCount
>
> 2.设置水平的标题   setHorizontalLabels
>
> 3.设置表格某行某列的数据  setItem(row,clo,item);

## 19.容器

```
	Stack WIdget:	页面切换需要自定义来完成
	ui->StackWidget->setCurrentIndex(0~x);	与数组一样，从零开始
```

## 20.其他

```
label 既可以设置静态，也可以显示动态图片（设置movie）
//设置label播放
	QMovie *movie = new QMovie("路径",QByteArrey(),this);
	ui->label_2->setMovie(movie);
	movie->start();
```

