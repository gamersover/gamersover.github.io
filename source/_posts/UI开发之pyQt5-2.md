---
title: UI开发之pyQt5二：
date: 2021-05-02 18:28:35
tags:
    - UI开发
    - Qt
    - python
categories: 教程
---

<!--
    开始声明并介绍Qt的版本关系
    1. 开局放一张Qt类继承的关系图，并表明不是为了记住，而是为了后面介绍组件的关系以及作为编码时可以进行查找的手册，所以看到这么复杂的图不要慌
    2. 然后依次介绍UI组成部分
    3. 介绍相应代码
-->

<!-- 每个控件的具体属性还是分开讲，并且要先讲控件再讲布局
### 文本显示类控件
#### 单行编辑框（QLineEdit）

单行编辑框一般是用来输入用户名或密码等简要内容的，其写法如下：
```python
import sys
from PyQt5.QtWidgets import QMainWindow, QApplication, QLineEdit, QVBoxLayout, QWidget


class MainWindow(QMainWindow):
    def __init__(self, parent=None):
        super(MainWindow, self).__init__(parent)   
        self.init_ui()
        self.show()

    def init_ui(self):
        self.setGeometry(1000, 400, 300, 300)
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


if __name__ == "__main__":
    app = QApplication(sys.argv[1:])

    window = MainWindow()
    window.show()

    sys.exit(app.exec_())
```
其中创建了一个主控件`main_widget`用来显示其他控件，使用`setCentralWidget(main_widget)`可以设置为主窗口`QMainWindow`的主控件，该控件使用了垂直布局方式`QVBoxlayout`，其中包含两个文本编辑子控件。`QLineEdit`可以设置的一些属性：
* `setPlaceholderText(a0: str)`：
    * 提示内容，输入时会覆盖
* `setText(self, atext: str)`：
    * 设置文本显示的内容
* `setMaxLength(self, a0: int)`
* `setEchoMode(self, a0: 'QLineEdit.EchoMode')`
    * 输入文本不可见，一般用在输入密码时保护隐私，以小圆点替代输入的文本来显示
* `setAlignment(self, flag: typing.Union[QtCore.Qt.Alignment, QtCore.Qt.AlignmentFlag])`
    * 文本的显示对齐方式，有居中对齐，左对齐，右对齐等
* `setReadOnly(self, a0: bool)`
    * 可知设置为只读模式，这时不能编辑
-->