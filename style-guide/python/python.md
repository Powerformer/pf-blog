# PEP8

全部内容请参考[Python官网](https://www.python.org/dev/peps/pep-0008/)。以下为需要关注的几点。

## 代码布局

### 缩进

使用 4 格缩进，只能使用空格，不能用 Tab。

### 空行的使用

- 最外层函数定义前后需要加两个空格。

```python
(blank_line)
(another_blank_line)
def top_level_function():
    pass
(blank_line)
(another_blank_line)
```

- 类的定义前后需要加两个空格。

```python
(blank_line)
(another_blank_line)
class SomeClass(object):
    pass
(blank_line)
(another_blank_line)
```

- 类中的方法前后加一个空格。

```python
class SomeClass(object):

    def method_one():
        pass

    def method_two():
      	pass

```

- 适度地使用空格对代码进行**逻辑分区**。

### 编码问题

- 不要使用编码声明（encoding declaration）。

- 命名标识符时，尽可能地使用ASCII范围内的字符（仅当测试非ASCII功能以及书写源码作者名时使用）：


###  `import` 语句

- 使用 `import ...` 语法，一次只能导入一个模块：

```python
# bad
import sys, os

# good
import os
import sys
```

而 `from ... import ...` 语法则应在一行中导入所有需要的语法元素：

```python
from subprocess import Popen, PIPE
```

- 多个 `import` 的组织顺序：**标准库 - 相关的第三方库 - 本地应用/特定库引入**。这三组 `import` 语句之间应当用一行空格隔开。对照以上规则，来看一个 Django 项目中的一个文件中 `import` 的使用。

```python
import uuid
from os.path import abspath

from django.db import models
from django.utils.translation import ugettext_lazy as _

from django_extensions.db.models import TimeStampedModel

from users.models import Profile
from home.models import Post
```

### 模块级双下划线名

模块级双下划线名（Module level dunder names），例如 `__all__`、`__author__`等，应当位于模块文档字符串（module docstring）之后，任何 `import` 语句之前（除了 `from __future__` ）。

```python
"""This is the example module.

This module does stuff.
"""

from __future__ import with_statement

__all__ = ['a', 'b', 'c']
__version__ = '1.0.0'
__author__ = 'Powerformer Inc.'

import os
import sys
```


## 命名规范

### 首要原则

命名应着重反映**用法**，而不是**实现方式**。

### 风格规定

- 变量、函数（方法）、模块和包的命名都应使用 `snake_case` ，即全部字母小写，用单下划线相连接。

```python
# bad
someValue = 100

# good
some_value = 100
```

- 类名应当使用 `PascalCase` ，即每个单词首字母大写。

```python
# bad
class Some_Class(object):
    pass

# good
class SomeClass(object):
    pass
```

-（全局）常量应当全部大写，并且用下划线相连。

```python
# bad
globalConstant = 0.75

# good
GLOBAL_CONSTANT = 0.75
```

## 空格的使用

### 使用工具消除行尾空格

- **避免**代码行末尾的空格。可以通过相应的编辑器插件轻松实现。

### 运算符两边的空格

- 在二元运算符两边加空格。

```python
# bad
i=i+1

# good
i = i + 1
```

- 当表达式中出现优先级不同的运算符时，在优先级最低的运算符两边加空格，其他运算符不要加。

```python
# bad
hypot2 = x * x + y * y

# good
hypot2 = x*x + y*y
```

### 避免多余空格

- 在定义函数关键词参数（或传入关键词参数）时加入空格。

```python
# bad
def complex(real, imag = 0.0):
    return magic(r = real, i = imag)

# good
def complex(real, imag=0.0):
    return magic(r=real, i=imag)
```

- 在括号符号内直接加入空格。

```python
# bad
spam( ham[ 1 ], { eggs: 2 } )

# good
spam(ham[1], {eggs: 2})
```

- 列表（或元组）最后一个元素的逗号后加入空格。

```python
# bad
foo = (0, )

# good
foo = (0,)
```

- 在逗号、冒号或分号之前加入空格。

```python
# bad
x , y = y , x

# good
x, y = y, x
```
- 切片语法中的多余空格。

```python
# bad
ham[lower + offset:upper + offset]
ham[1: 9], ham[1 :9], ham[1:9 :3]
ham[lower : : step]
ham[ : upper]

# good
ham[lower+offset : upper+offset]
ham[1:9], ham[1:9:3]
ham[lower::step]
ham[:upper]
```

- 函数调用中的多余空格。

```python
# bad
spam (1)

# good
spam(1)
```

- 索引中的多余空格。

```python
# bad
dct ['key'] = lst [index]

# good
dct['key'] = lst[index]
```

- 为了对齐赋值运算符而产生的多余空格。

```python
# bad
x             = 1
y             = 2
long_variable = 3

# good
x = 1
y = 2
long_variable = 3
```

## 注释

### 首要原则

- 永远保证注释和代码是同步更新的。

- 注释应当是完整的句子，第一个字母应当大写（除非开头是一个全部小写的标识符），`#`与注释语句之间应包含一个空格。

```python
# This should be a complete sentence.
some_object.do_something()
```

- 用英语书写注释。关于规范，参考 *The Elements of Style* (Strunk, W., Jr. and White, E.B.)。

### 块级注释

```python
# Lorem ipsum dolor sit amet consectetur adipisicing elit. Totam
# quaerat nam laborum quibusdam deserunt! Alias, vel natus? Quae
# architecto quia consectetur adipisci culpa nisi sapiente nihil
# distinctio recusandae tempora.
#
# Harum reiciendis quas nam adipisci dolor quasi, possimus vitae
# quaerat nemo ad? Placeat eligendi non eum quidem accusamus
# voluptas perferendis et!
do_something()
```

- 块级注释用于解释其后所有（或部分）的代码，并且与被注释的代码保持相同的缩进等级。

- 每一行内容都以 `#` 和**一个空格**开头（除非此行在注释内是需要缩进的）。

- 块级注释内的段落之间如果有空行，那么此行仅需包含一个 `#` 。

### 内联注释

```python
inline_comment = "This is an inline comment."   # Use it sparingly
```

- 尽量减少内联注释的使用。

- 注释应当与被注释的语句相隔**至少两个空格**。

- 不要写显而易见的注释。

```python
# bad
x = x + 1   # Increment x

# good
x = x + 1   # Compensate for border
```

### 文档字符串

- 为**所有**公共模块、函数、类和方法写文档字符串。对于私有方法来说，文档字符串不是必要的，但是应该有一些注释来表明其功能。

- 如果文档字符串有多行，那么结尾的 `"""` 应当单独在最后一行。

```python
# Don't do this.
"""Lorem ipsum dolor sit amet consectetur adipisicing elit. Esse
molestias ut qui quidem ad nihil? Cumque tenetur maiores excepturi
laboriosam."""

# This is good.
"""Lorem ipsum dolor sit amet consectetur adipisicing elit. Esse
molestias ut qui quidem ad nihil? Cumque tenetur maiores excepturi
laboriosam.
"""

# This is also good.
"""
Lorem ipsum dolor sit amet consectetur adipisicing elit. Esse
molestias ut qui quidem ad nihil? Cumque tenetur maiores excepturi
laboriosam.
"""
```

- 如果文档字符串仅有一行，那么结尾的 `"""` 应当在同一行。

```python
# bad
"""
Lorem ipsum dolor sit amet.
"""

# good
"""Lorem ipsum dolor sit amet."""
```
