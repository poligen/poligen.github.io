---
title: 什麼是if __name__ = "__main__" 啊？？
date: 2016-09-21 22:03:48
tags: python, 豆知識
categories: python
---
## if __name__ == __main__

之前學Flask 常常會看到這一段的code, 但不知所以然。本著得過且過的心態。但這個問題一直困擾著我。其實google 一下就會有很多解答了喔。不過來看一下Python document 是怎麼說的：

> The __name__ attribute must be set to the fully-qualified name of the module. This name is used to uniquely identify the module in the import system.

那關於__mian__呢？

> '__main__' is the name of the scope in which top-level code executes. A module’s __name__ is set equal to '__main__' when read from standard input, a script, or from an interactive prompt.

也就是說，所以有的module都有一個內建的__name__的屬性。

那什麼是['__main__'](https://docs.python.org/3/library/__main__.html)呢？
是用來檢查這段code是不是import而來的，如果是import來的，這裡的__name__ attribute就不會維持'__main__'而是其他的了。

舉個[例子](https://github.com/mjhea0/thinkful-mentor/tree/master/python/main_routine)來說好了:
```python
# first.py
if __name__ == '__main__':
  print('這是來自本來的module~~')
else:
  print('這是別人家來的!!')
```

```python
# second.py
import first
```

```
$python first.py
這是來自本來的module~~
$python second.py
這是別人家來的!!
```
