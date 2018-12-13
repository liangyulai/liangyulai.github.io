---
layout:     post
title:      "Python C++ 扩展"
subtitle:   ""
date:       2018-11-19 19:00
author:     "Liang Yulai"
header-img: "img/post-bg-2018.jpg"
tags:
    - Python C++
---

# 需求

    * Python 模块编译为 so 文件
    * Python 调用该 so 文件（C 也可以调用该 so 文件）

# 准备工作：安装 Cpython

```bash
pip install python-dev cpthon
```

# 创建 Cpython 文件： hello.pyx

```
kirin@kawagarbo:~/workspace/test$ tree
.
├── hello.pyx
├── setup.py
└── test.py

```

### hello.pyx

```python
class ctest:

    def print_hello(self):
        print "hello"

    def print_your_name(self, name):
        print name
```

### setup.py

```python
from distutils.core import setup
from Cython.Build import cythonize

setup(
        ext_modules = cythonize("hello.pyx")
)
```

### test.py

```python
#! /usr/bin/python2
from hello import ctest # import hello.so

def main(name):
    ctest().print_hello()
    ctest().print_your_name(name)

if __name__ == "__main__":
    main("Alex")
```


## 编译 pyx 文件为 so 文件

```bash
$ python setup.py build_ext --inplace
```

## 编译结果

```
kirin@kawagarbo:~/workspace/test$ tree
.
├── build
│   └── temp.linux-x86_64-2.7
│       └── hello.o
├── hello.c
├── hello.pyx
├── hello.so
├── setup.py
└── test.py
```

## 测试一下

```
kirin@kawagarbo:~/workspace/test$ ./test.py
hello
Alex
```

## 

[Cython](https://cython.org/)
[Cython@Github](https://github.com/cython/cython)
[Cython by Kurt W. Smith](https://www.safaribooksonline.com/library/view/cython/9781491901731/)
[Cython examples](https://github.com/cythonbook/examples)

