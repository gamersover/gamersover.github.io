---
title: UI开发之PyQt5一：控件和布局的关系
date: 2021-04-16 23:29:31
tags:
    - UI开发
    - Qt
    - python
categories: 教程
---

## 介绍
PyQt5是一个用来开发UI的python第三方库，UI不外乎显示控件，布局与交互，简单说就是确定UI需要展示哪些东西，这些东西如何排布在界面上以及操作这些东西的逻辑是什么。而PyQt5的控件与布局几乎都在QtWidgets这个类里面，至于交互就是后面所要讲的信号与槽。

<!--more-->

### 显示控件
PyQt5要显示控件，需要依附在顶层类中，这些顶层类作为主入口来显示其他控件，而顶层显示类大概有三种：
1. **QMainWindow**
最常用的顶层显示类，具有菜单栏、工具栏、状态栏、标题栏等
2. **QDialog**
一般用在对话窗口，属于非长时间展示的窗口，没有菜单栏，工具栏等
3. **Qwidget**
所有显示控件的父类，可以作为UI显示的主窗口，也可以作为子窗口嵌入到其他控件，比较灵活

以上三个类都在`PyQt5.QtWidgets`类下面。

常用显示控件有：

* `QMenu`：菜单栏
* `QLineEdit`：单行文本编辑框
* `QTextEdit`：多行文本编辑框
* `QProgressBar`：进度条
* `QPushButton`：按钮
* `QRadioButton`：单选按钮
* `QCheckBox`：复选按钮
* `QLable`：标签类，用来显示文本或图片

以上控件也在`PyQt5.QtWidgets`类下面。

### 布局
一个父控件下面可以包含许多子控件，而子控件的显示方式就依赖于布局，PyQt5中的布局方式有很多，这里先简单介绍几种：
1. **QHBoxLayout**
    水平显示布局，所有的控件水平排列
2. **QVBoxLayout**
    垂直显示布局，所有的控件垂直排列
3. **QGridLayout**
    网格布局，可以指定控件的具体位置（哪行哪列）

以上布局在`PyQt5.QtWidgets`类下面。

PyQt5控件布局的基本逻辑是：
1. 父控件假设为`pWidget`，
```python
    pWidget = QWidget()
```
2. 给该控件添加一种布局`layout`
```python
    pWidget.setLayout(layout)
```
3. 在layout里面添加子控件
```python
    layout.addWidget(widget1)
    layout.addWidget(widget2)
```

## 例子

### 主窗口写法
界面展示：
<img src="https://jsd.cdn.zzko.cn/gh/gamersover/hexo_blog_assets@main/pyqt教程/No1.jpg" width="25%">
代码如下：

```python
import sys
from PyQt5.QtWidgets import QMainWindow, QApplication


class MainWindow(QMainWindow):
    def __init__(self, parent=None):
        super(MainWindow, self).__init__(parent)
        self.init_ui()
        self.show()

    def init_ui(self):
        self.setGeometry(200, 200, 1600, 800)
        self.setWindowTitle("主窗口")

if __name__ == "__main__":
    app = QApplication(sys.argv[1:])

    window = MainWindow()
    window.show()

    sys.exit(app.exec_())
```

可以看到初始化了一个主窗口`QMainWindow`，其中Qt程序的入口使用`QApplication`类实现，主窗口的属性如下：

|  | API                                             | 描述                                                         |
| :------------- | :---------------------------------------------- | :----------------------------------------------------------- |
|        窗口大小与位置        | setGeometry(ax: int, ay: int, aw: int, ah: int) | 统一设置窗口的显示位置和大小，ax,ay表示窗口左上角坐标，aw,ah表示窗口的宽和高，都是以像素为单位 |
|                | move(ax: int, ay: int)                          | 单独设置窗口的显示位置即左上角坐标                           |
|                | setFixedSize(w: int, h: int)                    | 单独设置窗口的大小即宽和高                                   |
|                | setFixedWidth(w: int)                           | 单独设置窗口的宽                                             |
|                | setFixedHeight(h: int)                          | 单独设置窗口的高                                             |
| 窗口标题与图标 | setWindowTitle(a0: str)                         | 设置窗口标题                                                 |
|                | setWindowIcon(icon: QIcon)                      | 设置窗口图标                                                 |
| 控件显示       | show()                                          | 显示控件与其子控件                                           |



### 设置布局和添加控件
有了主窗口后，就可以添加想要添加的控件了，如下面代码：

```python
def init_ui(self):
    self.setGeometry(200, 200, 1600, 800)
    self.setWindowTitle("主窗口")

    main_widget = QWidget()
    main_layout = QVBoxLayout()
    main_widget.setLayout(main_layout)
    line_edit1 = QLineEdit()
    line_edit1.setPlaceholderText("用户名")
    line_edit2 = QLineEdit()
    line_edit2.setPlaceholderText("密码")
    main_layout.addWidget(line_edit1)
    main_layout.addWidget(line_edit2)

    self.setCentralWidget(main_widget)
```

其中创建了一个主控件`main_widget`用来显示其他控件，使用`setCentralWidget(main_widget)`可以将`main_widget`设置为主窗口`QMainWindow`的中央控件，该控件使用了垂直布局方式`QVBoxlayout`，其中包含两个文本编辑子控件`QLineEdit`。从这里也可以看出widget和layout的关系：

> widget.setLayout(layout)
> layout.addWidget(widget)

`widget`通过`setLayout`方法添加布局，而`layout`通过`addWidget`方法添加子控件。

### 控件的嵌套
当需要实现复杂的UI界面时，控件之间嵌套必不可少，比如要实现下图中的界面：
<img src="https://jsd.cdn.zzko.cn/gh/gamersover/hexo_blog_assets@main/pyqt教程/No3.jpg" width="25%">
可以先整理下该界面中的有哪些控件以及包含关系：
<img src="https://jsd.cdn.zzko.cn/gh/gamersover/hexo_blog_assets@main/pyqt教程/No2.jpg" width="60%">
其中主控件(`main_widget`)包含左右两个控件，而右边控件(`right_widget`)又包含两个控件，所以代码实现为：

```python
import sys
from PyQt5.QtWidgets import QMainWindow, QApplication, QVBoxLayout, QWidget, QHBoxLayout


class MainWindow(QMainWindow):
    def __init__(self, parent=None):
        super(MainWindow, self).__init__(parent)
        self.init_ui()
        self.show()

    def init_ui(self):
        self.setStyleSheet("border:2px solid black")
        self.setGeometry(1000, 400, 300, 300)
        self.setWindowTitle("主窗口")

        main_widget = QWidget()
        self.setCentralWidget(main_widget)

        main_layout = QHBoxLayout()
        main_widget.setLayout(main_layout)

        left_widget = QWidget()
        right_widget = QWidget()
        main_layout.addWidget(left_widget)
        main_layout.addWidget(right_widget)

        right_layout = QVBoxLayout()
        right_widget.setLayout(right_layout)

        top_widget = QWidget()
        buttom_widget = QWidget()
        right_layout.addWidget(top_widget)
        right_layout.addWidget(buttom_widget)


if __name__ == "__main__":
    app = QApplication(sys.argv[1:])

    window = MainWindow()
    window.show()

    sys.exit(app.exec_())
```

`main_widget`使用了`QHBoxLayout`布局方式，包含`left_widget`和`right_widget`两个子控件；而`right_widget`使用了`QVBoxLayout`布局方式，包含了`top_widget`和`buttom_widget`两个子控件。
