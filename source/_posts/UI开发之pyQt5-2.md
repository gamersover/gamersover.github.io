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

### QLineEdit（单行文本编辑框）

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

|              | API                                                                            | 描述                                                                                                                           |
| :----------- | :----------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------- |
| 设置文本属性 | setText(text: str)                                                             | 设置显示的文本                                                                                                                 |
|              | setMaxLength(a0: int)                                                          | 设置文本最大长度                                                                                                               |
|              | setEchoMode(a0: 'QLineEdit.EchoMode')                                          | 输入文本不可见，一般用在输入密码时保护隐私，以小圆点替代输入的文本来显示                                                       |
|              | setAlignment(flag: typing.Union[QtCore.Qt.Alignment, QtCore.Qt.AlignmentFlag]) | 文本的显示对齐方式，有居中对齐，左对齐，右对齐等                                                                               |
|              | setReadOnly(a0: bool)                                                          | 可知设置为只读模式，这时不能编辑                                                                                               |
|              | setPlaceholderText(a0: str)                                                    | 设置占位提示文本                                                                                                               |
|              | setCompleter(completer: QCompleter)                                            | 设置文本自动补全                                                                                                               |
|              | setFont(a0: QtGui.QFont)                                                       | 设置文本字体                                                                                                                   |
| 获取文本状态 | text()->str                                                                    | 获取文本                                                                                                                       |
|              | displayText()->str                                                             | 获取显示的文本，和显示的内容保持一致。如果是普通文本，则和`text()`返回的值一样；如果设置了`EchoMode`为密码类型，则返回小圆点。 |
|              | placeholderText()->str                                                         | 获取占位提示文本                                                                                                               |
| 操作文本内容 | insert(a0: str)                                                                | 在当前光标后插入文本                                                                                                           |
|              | clear()                                                                        | 删除所有内容                                                                                                                   |
|              | setClearButtonEnabled(enable: bool)                                            | 添加文本清除按钮                                                                                                               |
|              | selectAll()                                                                    | 选中所有内容                                                                                                                   |
| 其他         | setFocus()                                                                     | 设置为焦点（即当前操作对象）                                                                                                   |


### QTextEdit（多行文本编辑框）

展示
<img src="https://raw.githubusercontent.com/gamersover/hexo_blog_assets/main/pyqt%E6%95%99%E7%A8%8B/No5.gif" width="25%">

该控件用来实现大量文本的输入与显示，写法如下：

```python
text_edit = QTextEdit()
text_edit.setPlaceholderText("请输入")
```

基本属性有：

|              | API                                                                                  | 描述                                                                                                |
| :----------- | :----------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------- |
| 设置文本属性 | setPlainText(text: str)                                                              | 设置普通文本                                                                                        |
|              | setHtml(text: str)                                                                   | 设置html文本                                                                                        |
|              | setText(text: str)                                                                   | 设置文本，根据text参数判断为普通文本还是html文本，不建议使用，而是直接使用`setPlainText`或`setHtml` |
|              | setMarkdown(markdown: str)                                                           | 设置markdown文本                                                                                    |
|              | setAlignment(self, flag: typing.Union[QtCore.Qt.Alignment, QtCore.Qt.AlignmentFlag]) | 文本的显示对齐方式，有居中对齐，左对齐，右对齐等                                                    |
|              | setReadOnly(self, a0: bool)                                                          | 可知设置为只读模式，这时不能编辑                                                                    |
|              | setPlaceholderText(placeholderText: str)                                             | 设置占位提示文本                                                                                    |
|              | setLineWrapMode(mode: 'QTextEdit.LineWrapMode')                                      | 当参数为`NoWrap`时，文本长度宽度后不自动换行显示，这时通过拖动水平滚动条查看超出的内容              |
|              | setFont(a0: QtGui.QFont)                                                             | 设置文本字体                                                                                        |
|              | setTextColor(c: typing.Union[QtGui.QColor, QtCore.Qt.GlobalColor, QtGui.QGradient])  | 设置字体颜色                                                                                        |
| 获取文本状态 | toPlainText()->str                                                                   | 将显示文本转换为普通文本并返回                                                                      |
|              | toHtml()->str                                                                        | 将显示文本转换为html文本并返回                                                                      |
|              | toMarkdown()->str                                                                    | 将显示文本转换为markdown文本并返回                                                                  |
| 操作文本内容 | insertHtml(text: str)                                                                | 在当前光标后插入html文本                                                                            |
|              | insertPlainText(text: str)                                                           | 在当前光标后插入普通文本                                                                            |
|              | clear()                                                                              | 清除所有内容                                                                                        |
|              | selectAll()                                                                          | 选中所有内容                                                                                        |
| 其他         | setFocus()                                                                           | 设置为焦点（即当前操作对象）                                                                        |
### 字体和字体颜色

文本类肯定离不开字体以及字体颜色，而Qt5的字体类型与字体颜色是分开的，都是在`QtGui`类中

#### 字体

字体在`QtGui.QFont`类中，初始化方法
```python
font = QFont(family: str, pointSize: int, weight: int, italic: bool)
```
* family：字体风格
* pointSize：字体大小，可选参数
* weight：字体粗细，可选参数
* italic：是否斜体，可选参数

基本属性有：

|              | API                                                         | 描述                                |
| :----------- | :---------------------------------------------------------- | :---------------------------------- |
| 设置文本属性 | setWordSpacing(spacing: float)                              | 设置文本间距                        |
|              | setBold(enable: bool)                                       | 设置文本加粗                        |
|              | setFamily(a0: str)                                          | 设置字体风格                        |
|              | setPointSize(a0: int)                                       | 设置文本大小（字号单位）            |
|              | setPixelSize(a0: int)                                       | 设置文本大小（像素单位）            |
|              | setWeight(a0: int)                                          | 设置文本粗细，比`setBold`细粒度设置 |
|              | setItalic(b: bool)                                          | 设置斜体                            |
|              | setOverline(a0: bool)                                       | 设置字体上划线                      |
|              | setunderline(a0: bool)                                      | 设置字体下划线                      |
|              | setLetterSpacing(type: 'QFont.SpacingType', spacing: float) | 设置字间距                          |
| 获取文本属性 | family() -> str                                             | 获取字体风格                        |
|              | pointSize() -> int                                          | 获取字体大小（字号单位）            |
|              | pixelSize() -> int                                          | 获取文本大小（像素单位）            |



比如对`QLineEdit`设置字体：
```python
line_edit = QLineEdit()
font = QFont("Arial", 20)
# 设置字间距
font.setLetterSpacing(QFont.SpacingType.AbsoluteSpacing, 10)
line_edit.setFont(font)
```

#### 颜色
字体在`QtGui.QColor`类中，初始化方法有
1. `QColor(rgb: int)`
2. `QColor(r: int, g: int, b: int, alpha: int)`：alpha表示透明度，可选参数，取值0-255，0表示完全透明，255表示无透明
3.  `QColor(self, aname: str)`

比如初始化一个红色
```python
color = QColor("red")
color = QColor(0xff00ff)
color = QColor(255, 0, 0)
```

基本属性有：

|          | API                                                 | 描述                             |
| :------- | :-------------------------------------------------- | :------------------------------- |
| 设置颜色 | setCmyk(c: int, m: int, y: int, k: int, alpha: int) | 设置颜色（cmyk格式）             |
|          | setHsl(h: int, s: int, l: int, alpha: int)          | 设置颜色（hsl格式）              |
|          | setHsv(h: int, s: int, v: int, alpha: int)          | 设置颜色（hsv格式）              |
|          | setAlpha(alpha: int)                                | 设置颜色透明度                   |
|          | darker(factor: int)                                 | 根据factor参数返回一个更深的颜色 |
|          | lighter(factor: int)                                | 根据factor参数返回一个更浅的颜色 |
| 获取颜色 | getCmyk() -> Tuple[int, int, int, int, int]         | 获取颜色（cmyk格式）             |
|          | getHsl() -> Tuple[int, int, int, int]               | 获取颜色（hsl格式）              |
|          | getHsv() -> Tuple[int, int, int, int]               | 获取颜色（hsv格式）              |
|          | getRgb() -> Tuple[int, int, int, int]               | 获取颜色（rgb格式）              |


比如对`QTextEdit`设置字体：
```python
text_edit = QTextEdit()
color = QColor("red")
text_edit.setTextColor(color)
```

！注意: `QLineEdit`并没有`setTextColor`方法，所以不能直接设置颜色，这时需要使用更加强大的`QSS`来配置，类似于网页设计中的`CSS`。后续章节会详细介绍。