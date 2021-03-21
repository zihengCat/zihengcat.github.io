---
title: LeetCode - 504. 转换7进制（Base 7）
date: 2018-03-01
tags: [Algorithm, LeetCode]
---

# 前言

本文记录*LeetCode - 504. 转换7进制（Base 7）*问题。


# 问题描述

输入一个**10进制**整型数，将该数的**7进制**以字符串形式返回。

**例1：**

```
输入: 100
输出: "202"
```

**例2：**

```
输入: -7
输出: "-10"
```

**注：**输入数字的范围是*[-1e7, 1e7]*。

# 问题解法

进制转换的问题，算法就是整除取余数。

**例如：**

```
1: 101 / 7 = 14, 100 % 7 = 3
2: 14  / 7 = 2,  14  % 7 = 0
3: 2   / 7 = 0,  2   % 7 = 2
```

| 迭代轮数 | 商  | 余数 |
| -------- | --- | ---- |
| 1        | 14  | 3    |
| 2        | 2   | 0    |
| 3        | 0   | 2    |

> 将十进制数101转换为七进制

当被除数被除到0时，把取出的余数按**从后至前**的顺序排列，就可以得到进制转换后的数。
对于负数，可以先将负数转换为相反数进行运算，最后再加回负号。

```
#include <string>
#include <algorithms>
using namespace std;
/* ... */
string convertToBase7(int num) {
    /* 处理负数 */
    bool flag = false;
    if(num < 0) {
        num *= -1;
        flag = true;
    }

    int remainder = 0;
    string ret("");

    /* 算法至少要执行1次 */
    do {
        remainder = num % 7;
        num /= 7;
        ret += to_string(remainder);
    } while(num);

    /* 注意排列顺序 */
    reverse(ret.begin(), ret.end());

    /* 加回负号 */
    return flag == true ? "-" + ret : ret;
}
```

> C++ 代码 - convertToBase7

如果使用C的话，我们需要手动实现一下`to_string`与`reverse`函数。C++有标准库可以直接使用。

# 拓展延伸

理解了进制转换的算法后，我们就可以尝试将代码改写成支持转换**任意进制**数。

```
string convertToBaseN(int num, int base) {
    bool flag = false;
    if(num < 0) {
        num *= -1;
        flag = true;
    }
    string ret("");
    int remainder = 0;
    do {
        remainder = num % base;
        num /= base;
        /* 使用自定义的字符串转换函数 */
        ret += typical_to_string(remainder);
    } while(num);
    reverse(ret.begin(), ret.end());
    return flag == true ? "-" + ret : ret;
}
```

> C++ 代码 - convertToBaseN

这里我们需要自定义字符串转换函数，如要转换为`16进制`，就需要把余数`10`转换为字符串`"a"`，余数`11`转换为字符串`"b"`...依此类推。这个字符串转换函数需要我们手动编写。

