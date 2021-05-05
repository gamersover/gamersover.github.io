---
title: UI开发之PyQt5二：文本类控件
date: 2021-05-02 18:28:35
tags:
    - UI开发
    - Qt
    - python
categories: 教程
---

文本类控件主要包含：
* QLineEdit：单行文本编辑框
* QTextEdit：多行文本编辑框

<!--more-->

#### QLineEdit（单行文本编辑框）

展示
<img src="https://raw.githubusercontent.com/gamersover/hexo_blog_assets/main/pyqt%E6%95%99%E7%A8%8B/No4.gif" width="25%">

该控件一般用来输入用户名或密码等简要内容，其写法如下：
```python
line_edit = QLineEdit()
line_edit.setPlaceholderText("密码")
line_edit.setClearButtonEnabled(True)
line_edit.setEchoMode(QLineEdit.EchoMode.Password)
```
其基本属性包含：

|              | API                                                          | 描述                                                         |
| :----------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 设置文本属性 | setText(text: str)                                    | 设置显示的文本                                               |
|              | setMaxLength(a0: int)                                  | 设置文本最大长度                                             |
|              | setEchoMode(a0: 'QLineEdit.EchoMode')                  | 输入文本不可见，一般用在输入密码时保护隐私，以小圆点替代输入的文本来显示 |
|              | setAlignment(flag: typing.Union[QtCore.Qt.Alignment, QtCore.Qt.AlignmentFlag]) | 文本的显示对齐方式，有居中对齐，左对齐，右对齐等             |
|              | setReadOnly(a0: bool)                                  | 可知设置为只读模式，这时不能编辑                             |
|              | setPlaceholderText(a0: str)                                  | 设置占位提示文本                                             |
|              | setCompleter(completer: QCompleter)                          | 设置文本自动补全                                             |
| 获取文本状态 | text()->str                                                  | 获取文本                                                     |
|              | displayText()->str                                           | 获取显示的文本，和显示的内容保持一致。如果是普通文本，则和`text()`返回的值一样；如果设置了`EchoMode`为密码类型，则返回小圆点。 |
|              | placeholderText()->str                                       | 获取占位提示文本                                             |
| 操作文本内容 | insert(a0: str)                                              | 在当前光标后插入文本                                         |
|              | clear()                                                      | 删除所有内容                                                 |
|              | setClearButtonEnabled(enable: bool)                          | 添加文本清除按钮                                             |


#### QTextEdit（多行文本编辑框）

展示
<img src="https://raw.githubusercontent.com/gamersover/hexo_blog_assets/main/pyqt%E6%95%99%E7%A8%8B/No5.gif" width="25%">

该控件用来实现大量文本的输入与显示，写法如下：

```python
text_edit = QTextEdit()
text_edit.setPlaceholderText("请输入")
```

基本属性有：

|              | API                                                          | 描述                                                         |
| :----------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 设置文本属性 | setPlainText(text: str)                                      | 设置普通文本                                                 |
|              | setHtml(text: str)                                           | 设置html文本                                                 |
|              | setText(text: str)                                           | 设置文本，根据text参数判断为普通文本还是html文本，不建议使用，而是直接使用`setPlainText`或`setHtml` |
|              | setMarkdown(markdown: str)                                   | 设置markdown文本                                             |
|              | setAlignment(self, flag: typing.Union[QtCore.Qt.Alignment, QtCore.Qt.AlignmentFlag]) | 文本的显示对齐方式，有居中对齐，左对齐，右对齐等             |
|              | setReadOnly(self, a0: bool)                                  | 可知设置为只读模式，这时不能编辑                             |
|              | setPlaceholderText(placeholderText: str)                     | 设置占位提示文本                                             |
|              | setLineWrapMode(mode: 'QTextEdit.LineWrapMode')              | 当参数为`NoWrap`时，文本长度宽度后不自动换行显示，这时通过拖动水平滚动条查看超出的内容 |
| 获取文本状态 | toPlainText()->str                                           | 将显示文本转换为普通文本并返回                               |
|              | toHtml()->str                                                | 将显示文本转换为html文本并返回                               |
|              | toMarkdown()->str                                            | 将显示文本转换为markdown文本并返回                           |
| 操作文本内容 | insertHtml(text: str)                                        | 在当前光标后插入html文本                                     |
|              | insertPlainText(text: str)                                   | 在当前光标后插入普通文本                                     |
|              | clear()                                                      | 清除所有内容                                                 |

#### 字体与颜色

文本类肯定离不开字体与颜色，PyQt5的字体与颜色分别在

##### 字体