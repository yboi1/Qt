## QT::事件

#### 事件分发机制

event 接受

```
bool Label::event(QEvent *e)
{
    if(e->type()==QEvent::MouseMove)
    {
        this->mouseMoveEvent(static_cast<QMouseEvent *>(e));
        return true;
    }
    return QLabel::event(e);
}
```



#### 事件

> 当某个事件（鼠标、键盘）发生的时候，窗口就会收到这个事件，并且调用相应的事件处理函数，
> 	事件处理函数的命名都是以Event结尾的事件先会到达窗口的event函数 ， event函数 返回值：true表示该事件得到处理，如果是false，没被处理，事件会继续传递到父窗口
> 通过判断QEvent 参数的类型得知事件的类型后，调用相应的事件处理函数
> 	
>
> 		//判断event的类型
> 		if(e->type()==QEvent::MouseMove)
> 		{
> 			this->mouseMoveEvent(static_cast<QMouseEvent *>(e));
> 			return true;
> 		}
> 		//其他没有被处理的事件，就使用父类的event
> 		return QLabel::event(e);



#### 鼠标事件

```
#include "label.h"
#include<QMouseEvent>

Label::Label(QWidget *parent) : QLabel(parent)
{
    this->setMouseTracking(true);    //保证鼠标进入文本框后自动显示
}

void Label::mousePressEvent(QMouseEvent *ev)
{
    //输出鼠标事件的信息
    //坐标
    int x = ev->x();
    int y = ev->y();
    //鼠标案件
    Qt::MouseButton btn =  ev->button();
    QString strButton = "";
    if(btn==Qt::LeftButton)
    {
        strButton = "LeftBUtton";
    }
    if(btn==Qt::RightButton)
    {
        strButton = "RightBUtton";
    }
    if(btn==Qt::MidButton)
    {
        strButton = "MidBUtton";
    }
    //Label可以显示html
    QString str = QString("<h1><center>press[%1,%2][%3]</center></h1>").arg(x).arg(y).arg(strButton);
    this->setText(str);
}

void Label::mouseMoveEvent(QMouseEvent *ev)
{
    //输出鼠标事件的信息
    //坐标
    int x = ev->x();
    int y = ev->y();
    //鼠标案件
    Qt::MouseButtons btns =  ev->buttons();		//buttons 可能同时点击多个按键
    QString strButton = "";
    if(btns & Qt::LeftButton)
    {
        strButton += "LeftBUtton;";
    }
    if(btns & Qt::RightButton)
    {
        strButton += "RightBUtton;";
    }
    if(btns & Qt::MidButton)
    {
        strButton += "MidBUtton";
    }
    //Label可以显示html
    QString str = QString("<h1><center>move[%1,%2][%3]</center></h1>").arg(x).arg(y).arg(strButton);	//Label的显示方式
    this->setText(str);
}
```

#### 定时器事件

