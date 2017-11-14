# PEP8

全部内容请参考[Python官网](https://www.python.org/dev/peps/pep-0008/)。以下为需要关注的几点。

## 代码布局

### 缩进

使用 4 格缩进，只能使用空格，不能用 Tab。

### 空行的使用

- 最外层函数定义前后需要加两个空格。

```python
import os


def top_level_function():
    pass


def another_function():
    pass


```

- 类的定义前后需要加两个空格。

```python
from django import db


class SomeClass(object):
    pass


class AnotherClass(object):
    pass


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

- 命名标识符时，尽可能地使用ASCII范围内的字符（仅当测试非ASCII功能以及书写源码作者名时使用）。


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

- 变量、函数（方法）、模块和包的命名都应使用 `snake_case` ，即全部字母小写，用单下划线相连接。需要注意的是，*模块名应当尽量不用下划线*，因为会让人误解成变量。

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

# PEP20（Python之禅）

输入 `import this` 即可查看 *The Zen of Python*（Python之禅）的全部内容。

以下代码实例均摘自 [Hunter Blanks 的演讲](http://artifex.org/~hblanks/talks/2011/pep20_by_example.pdf)。

## 美观胜于丑陋

不要盲目追求代码的简洁而牺牲可读性，这会让代码变得“丑陋”。

```python
"""
Give me a function that takes a list of numbers and returns only the
even ones, divided by two.
"""
# bad
halve_evens_only = lambda nums: map(lambda i: i/2, filter(lambda i: not i%2, nums))

# good
def halve_evens_only(nums):
    return [i/2 for i in nums if not i % 2]
```

## 显式胜于隐式

不要依赖所谓的“惯例”，让从来没有读过你代码的人也能知道你在做什么。

```python
"""
Load the cat, dog, and mouse models so we can edit instances of them.
"""
# bad
def load():
    from menagerie.cat.models import *
    from menagerie.dog.models import *
    from menagerie.mouse.models import *

# good
def load():
    from menagerie.models import cat as cat_models
    from menagerie.models import dog as dog_models
    from menagerie.models import mouse as mouse_models
```

## 简单胜于复杂

如果完成同一件事，可以用更简单的方法（例如选择更好的库），那就毫不犹豫地采用。

```python
"""
Can you write out these measurements to disk?
"""
measurements = [
    {'weight': 392.3, 'color': 'purple', 'temperature': 33.4},
    {'weight': 34.0, 'color': 'green', 'temperature': -3.1},
    ]

# bad
def store(measurements):
    import sqlalchemy
    import sqlalchemy.types as sqltypes

    db = sqlalchemy.create_engine('sqlite:///measurements.db')
    db.echo = False
    metadata = sqlalchemy.MetaData(db)
    table = sqlalchemy.Table('measurements', metadata,
        sqlalchemy.Column('id', sqltypes.Integer, primary_key=True),
        sqlalchemy.Column('weight', sqltypes.Float),
        sqlalchemy.Column('temperature', sqltypes.Float),
        sqlalchemy.Column('color', sqltypes.String(32)),
        )
    table.create(checkfirst=True)

    for measurement in measurements:
        i = table.insert()
        i.execute(**measurement)

# good
def store(measurements):
    import json
    with open('measurements.json', 'w') as f:
    f.write(json.dumps(measurements))
```

## 复杂胜于繁冗

如果不能做到用简单的方式解决问题，那就退而求其次用复杂的方法，但是不应让程序过于繁冗，结构应保持明确。

```python
"""
Can you write out those same measurements to a MySQL DB? I think we're
gonna have some measurements with multiple colors next week, by the way.
"""
# bad
def store(measurements):
    import sqlalchemy
    import sqlalchemy.types as sqltypes

    db = sqlalchemy.create_engine('sqlite:///measurements.db')
    db.echo = False
    metadata = sqlalchemy.MetaData(db)
    table = sqlalchemy.Table('measurements', metadata,
        sqlalchemy.Column('id', sqltypes.Integer, primary_key=True),
        sqlalchemy.Column('weight', sqltypes.Float),
        sqlalchemy.Column('temperature', sqltypes.Float),
        sqlalchemy.Column('color', sqltypes.String(32)),
        )
    table.create(checkfirst=True)

    for measurement in measurements:
        i = table.insert()
        i.execute(**measurement)

# good
def store(measurements):
    import MySQLdb
    db = MySQLdb.connect(user='user', passwd="password", host='localhost', db="db")

    c = db.cursor()
    c.execute("""
        CREATE TABLE IF NOT EXISTS measurements
          id int(11) NOT NULL auto_increment,
          weight float,
          temperature float,
          color varchar(32)
          PRIMARY KEY id
          ENGINE=InnoDB CHARSET=utf8
          """)

    insert_sql = (
        "INSERT INTO measurements (weight, temperature, color) "
        "VALUES (%s, %s, %s)")

    for measurement in measurements:
        c.execute(insert_sql,
            (measurement['weight'], measurement['temperature'], measurement['color'])
            )
```

## 减少嵌套的使用

```python
"""Identify this animal. """
# bad
def identify(animal):
    if animal.is_vertebrate():
        noise = animal.poke()
        if noise == 'moo':
            return 'cow'
        elif noise == 'woof':
            return 'dog'
    else:
        if animal.is_multicellular():
            return 'Bug!'
        else:
            if animal.is_fungus():
                return 'Yeast'
            else:
                return 'Amoeba'

# good
def identify(animal):
    if animal.is_vertebrate():
        return identify_vertebrate()
    else:
        return identify_invertebrate()

def identify_vertebrate(animal):
    noise = animal.poke()
    if noise == 'moo':
        return 'cow'
    elif noise == 'woof':
        return 'dog'

def identify_invertebrate(animal):
    if animal.is_multicellular():
        return 'Bug!'
    else:
        if animal.is_fungus():
            return 'Yeast'
        else:
            return 'Amoeba'
```

## 稀疏优于密集

适当地使用空行来划分代码段中的各个部分，会很大程度上增强可读性。这实际上运用了“亲密性”的设计原则（可参考[《写给大家看的设计书》](http://www.ituring.com.cn/book/1757)）。

```python
"""Parse an HTTP response object, yielding back new requests or data. """
# bad
def process(response):
    selector = lxml.cssselect.CSSSelector('#main > div.text')
    lx = lxml.html.fromstring(response.body)
    title = lx.find('./head/title').text
    links = [a.attrib['href'] for a in lx.find('./a') if 'href' in a.attrib]
    for link in links:
        yield Request(url=link)
    divs = selector(lx)
    if divs: yield Item(utils.lx_to_text(divs[0]))

# good
def process(response):
    lx = lxml.html.fromstring(response.body)

    title = lx.find('./head/title').text

    links = [a.attrib['href'] for a in lx.find('./a') if 'href' in a.attrib]
    for link in links:
        yield Request(url=link)

    selector = lxml.cssselect.CSSSelector('#main > div.text')
    divs = selector(lx)
    if divs:
        bodytext = utils.lx_to_text(divs[0])
        yield Item(bodytext)
```

## 重视可读性

可读性是 Python 语言的灵魂。

```python
""" Write out the tests for a factorial function. """
# bad
def factorial(n):
    """ Return the factorial of n, an exact integer >= 0.

    >>> [factorial(n) for n in range(6)]
    [1, 1, 2, 6, 24, 120]

    >>> factorial(30)
    265252859812191058636308480000000L

    >>> factorial(-1)
    Traceback (most recent call last):
    ...
    ValueError: n must be >= 0 """
    pass

if __name__ == '__main__' and '--test' in sys.argv:
    import doctest
    doctest.testmod()

# good
import unittest

def factorial(n):
    pass

class FactorialTests(unittest.TestCase):
    def test_ints(self):
        self.assertEqual(
            [factorial(n) for n in range(6)], [1, 1, 2, 6, 24, 120])

    def test_long(self):
        self.assertEqual(
            factorial(30), 265252859812191058636308480000000L)

    def test_negative_error(self):
        with self.assertRaises(ValueError):
            factorial(-1)

if __name__ == '__main__' and '--test' in sys.argv:
    unittest.main()
```

## 特例不应打破规则

即便特例有时会非常实用，但是也不应该破坏既定的规则。

```python
"""
Write a function that returns another functions. Also, test floating point.
"""
# bad
def make_counter():
    i = 0
    def count():
        """ Increments a count and returns it. """
        i += 1
        return i
    return count

count = make_counter()
assert hasattr(count, '__name__') # No anonymous functions!
assert hasattr(count, '__doc__')

assert float('0.20000000000000007') == 1.1 - 0.9 # (this is platform dependent)
assert 0.2 != 1.1 - 0.9 # Not special enough to break the rules of floating pt.
assert float(repr(1.1 - 0.9)) == 1.1 - 0.9

# good
def make_adder(addend):
return lambda i: i + addend # But lambdas, once in a while, are practical.

assert str(1.1 - 0.9) == '0.2' # as may be rounding off floating point errors
assert round(0.2, 15) == round(1.1 - 0.9, 15)
```

## 处理好每个错误

精确地捕获并处理所有异常，除非你有明确的理由不这样做。

```python
""" Import whatever json library is available. """
try:
    import json
except ImportError:
    try:
        import simplejson as json
    except:
        print 'Unable to find json module!'
        raise
```

## 消除可能的歧义

面对多种可能的情况时，不要（让别人）去猜测，应当消除各种可能的歧义。

```python
""" Store an HTTP request in the database. """
def process(response):
    db.store(url, response.body)

def process(response):
    charset = detect_charset(response)
    db.store(url, response.body.decode(charset))
```

## 解决问题的最好方法应当只有一种

也许并不容易一眼看出，但是在不断的思考和重构中会逐步接近那种最好的方法。除非你是龟叔（Guido van Rossum，Python 之父）。

## 做事之前多加考虑

做也许好过不做，但不假思索就动手还不如不做。

```python
def obsolete_func():
    raise PendingDeprecationWarning

def deprecated_func():
    raise DeprecationWarning
```

不假思索地实现功能，终究会使你的项目“动荡不安”，陷入 `Deprecation` 地狱。

## 让实现方法易于解释

很难解释的实现方法一定是个糟糕的方法，容易解释的实现方法可能是个好方法。

```python
def hard():
    import xml.dom.minidom
    document = xml.dom.minidom.parseString(
        ’’’<menagerie><cat>Fluffers</cat><cat>Cisco</cat></menagerie>’’’ )
    menagerie = document.childNodes[0]
    for node in menagerie.childNodes:
        if node.childNodes[0].nodeValue== ’Cisco’ and node.tagName == ’cat’:
            return node

def easy(maybe):
    import lxml
    menagerie = lxml.etree.fromstring(
        ’’’<menagerie><cat>Fluffers</cat><cat>Cisco</cat></menagerie>’’’ )
    for pet in menagerie.find(’./cat’):
    if pet.text == ’Cisco’:
        return pet
```

## 拥抱命名空间

```python
def chase():
    import menagerie.models.cat as cat
    import menagerie.models.dog as dog

    dog.chase(cat)
    cat.chase(mouse)
```

# Powerformer 推荐风格

## 续行

当出现很长的代码行时，首先考虑能否拆解成多个语句。如果不能，可以在很长的表达式两边加括号进行换行。不要使用 `\` 来续行，因为这较容易引入错误，并且不够美观。

### 字符串续行

```python
# bad
long_sentence = "Lorem ipsum dolor sit amet consectetur adipisicing \
elit. Veritatis blanditiis ex iure unde necessitatibus sequi qui \
iusto fuga illum facilis!"

# good
long_sentence = (
    "Lorem ipsum dolor sit amet consectetur adipisicing elit. "
    "Veritatis blanditiis ex iure unde necessitatibus sequi "
    "qui iusto fuga illum facilis!"
)
```

### `import` 续行

```python
# bad
from .serializers import CatSerializer, DogSerializer \
FrogSerializer, LionSerializer

# good
from .serializers import (
    CatSerializer,
    DogSerializer,
    FrogSerializer,
    LionSerializer
)
```
