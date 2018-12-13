---
layout:     post
title:      "signed integers and overflow"
subtitle:   "sub title(optional)"
date:       2018-11-21 19:00
author:     "Liang Yulai"
header-img: "img/post-bg-2018.jpg"
tags:
    - C++
---


## Signed integer overflow is **undefined behavior**

Specialization | std::numeric_limits<T>::min() | std::numeric_limits<T>::max()
numeric_limits<bool> | |
numeric_limits<char> | | 
numeric_limits<signed char> | |
numeric_limits<unsigned char> | |
numeric_limits<int> | |
numeric_limits<unsigned int> | | 
numeric_limits<long> | |
numeric_limits<unsigned long> | |
numeric_limits<long long> | |
numeric_limits<unsigned long long> | |
numeric_limits<float> | |
numeric_limits<double> | |
numeric_limits<long double> | |


## demo 

```cpp

```


## reference 
[INT32-C. Ensure that operations on signed integers do not result in overflow](https://wiki.sei.cmu.edu/confluence/display/c/INT32-C.+Ensure+that+operations+on+signed+integers+do+not+result+in+overflow)