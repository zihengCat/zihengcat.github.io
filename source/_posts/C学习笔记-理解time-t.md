---
title: C学习笔记-理解time_t
date: 2017-06-23
tags: C
---

# 前言

本文讲解C中的日期时间类型`time_t`。

## 时间的相关概念

在理解C的`time_t`类型之前，我们需要先认识一下时间的相关概念。

- **格林尼治标准时间(Greenwich Mean Time, 简称GMT)**
位于英国伦敦郊区的皇家格林尼治天文台当地的标准时间，本初子午线被定义为通过那里的经线。

- **世界协调时(Coordinated Universal Time, 简称UTC)**
以地球自转为基础的时间标准。由于地球自转速度并不均匀，导致了自转时间与世界时之间存在18个月就有1s的误差。为纠正这种误差，国际地球自转研究所根据地球自转的实际情况对格林威治时间进行增减闰秒的调整，与国际度量衡时间局联合向全世界发布标准时间，这就是所谓的世界协调时。

- **本地时间(Locale Time)**
整个地球分为二十四时区，由于时区的存在，每个时区都有自己的本地时间。

理论上来说，格林尼治标准时间的正午是指当太阳横穿格林尼治子午线时（也就是在格林尼治上空最高点时）的时间。但由于地球在它的椭圆轨道里的运动速度不均匀，这个时刻可能与实际的太阳时有误差，最大误差达16分钟。原因在于地球每天的自转是有些不规则的，而且正在缓慢减速，**因此格林尼治时间基于天文观测本身的缺陷，已经不再被作为标准时间使用。现在的标准时间，是由原子钟报时的世界协调时（UTC）来决定。**

# Unix时间的表示

我们可以粗略的认为，UTC时间等同与格林尼治时间。

在Unix系统中，使用**Unix时间戳**表示时间。
Unix时间戳(Unix timestamp)，也被称为称Unix时间(Unix time)、POSIX时间(POSIX time)，是一种时间表示方式，定义为**从格林尼治时间1970年01月01日00时00分00秒起至现在的总秒数。**Unix时间戳不仅被使用在Unix系统、类Unix系统中，也在许多其他操作系统中被广泛采用。

- 日历时间(Calendar Time)
用“从一个标准时间点到此时的时间经过的秒数”来表示的时间。这个标准时间点对不同的编译器来说会有所不同，但对一个编译系统来 说，这个标准时间点是不变的，该编译系统中的时间对应的日历时间都通过该标准时间点来衡量，所以可以说日历时间是“相对时间”，但是无论你在哪一个时区， 在同一时刻对同一个标准时间点来说，日历时间都是一样的。

- 时间点(Epoch)
时间点在标准C中是一个整数，它用此时的时间和标准时间点相差的秒数（即日历时间）来表示。

- 时钟计时单元(Clock Tick)
一个时钟计时单元的时间长短是由CPU控制的。一个clock tick不是CPU的一个时钟周期，而是C的一个基本计时单位。

# time\_t类型

`time_t`是C中表示时间的类型。其定义在头文件`time.h`之中。
但是，C标准并未对`time_t`实际类型作具体的定义，操作系统可以有不同的实现。
在Linux系统，我们可以在`/usr/include/bits/type.h`找到其实际的定义。(各种系统宏定义...很难找...)
更简单的方式，直接查看编译预处理后的`.i`文件。
```
...
typedef long int __time_t;
typedef __time_t time_t;
...
```
原来，`time_t`的真面目是`long int`，在64位机器上，占8个字节。
目前，大部分的操作系统都将`time_t`作为64位的整型实现，因为32位整型显得不太够用...

# tm结构体

我们已经知道，Unix时间戳是距*1970-01-01 00:00:00*的相对秒数,以64位整型`long int`表示。
可是只有相对秒数，显得不够不直观。Unix还提供了日期时间结构体`tm`。
```
struct tm {
  int tm_sec;    /* 秒 - [0 - 60] (多出1秒) */
  int tm_min;    /* 分 - [0 - 59] */
  int tm_hour;   /* 时 - [0 - 23] */
  int tm_mday;   /* 日 - [1 - 31] */
  int tm_mon;    /* 月 - [0 - 11] (月份从0开始，0代表一月) */
  int tm_year;   /* 年 - (实际年份 - 1900) */
  int tm_wday;   /* 星期 - [0 - 6] (0代表星期天，1代表星期一) */
  int tm_yday;   /* 今年的第几天 - [0 - 365] */
  int tm_isdst;  /* 夏令时标志 [-1/0/1] */

  long int tm_gmtoff;    /* 本地时距UTC时的秒数 */
  const char *tm_zone;   /* 时区 */
};
```
有了`tm结构体`，我们就可以方便的使用日期与时间啦。

# Unix时间函数

现在我们来看一看定义在头文件`time.h`中的一些时间日期相关的函数吧。

## time()

`time()`函数返回当前系统时间(Unix时间戳time\_t), 如果参数timer不为NULL，则也会把时间放入timer中。

```
/* 函数原型 */
extern time_t time (time_t *__timer)；
```
```
#include <stdio.h>
#include <time.h>
int main(void){
    time_t timer;
    timer = time(NULL); /* 得到当前系统时间(UTC) */
    printf("%ld seconds since the epoch began\n", timer);
    return 0;
}
```
## gmtime()

`gmtime()`函数可以将`time_t`转换成`tm结构体`。

```
/* 函数原型 */
extern struct tm *gmtime (const time_t *__timer)
```

```
#include <stdio.h>
#include <time.h>
int main(void){
    time_t timer;
    struct tm *ptm;
    timer = time(NULL);     /* 得到当前系统时间(UTC) */
    ptm = gmtime(&timer);   /* 转换为 tm 结构体 */
    return 0;
}
```
## localtime()

`localtime()`函数接收`time_t`, 返回本地时区的`tm结构体`。

```
/* 函数原型 */
extern struct tm *localtime (const time_t *__timer);
```

```
#include <stdio.h>
#include <time.h>

int main(void){
    time_t timer = time(NULL);

    /* time_t 转换为本地时区tm结构体后以字符串形式输出 */
    printf("%s\n", asctime(localtime(&timer)));
}

```

## asctime()

`asctime()`函数接收一个`tm结构体`，返回格式化字符串*Day Mon dd hh:mm:ss yyyy\n*。
```
/* 函数原型 */
extern char *asctime (const struct tm *__tp);
```

```
#include <stdio.h>
#include <time.h>

int main(void){
    time_t timer = time(NULL);

    /* time_t 转换为 tm 结构体后以字符串形式输出 */
    printf("%s\n", asctime(gmtime(&timer)));
}

```

## ctime()

`ctime()`函数接收一个`time_t`, 返回格式化字符串。等同于`asctime(localtime(&timer))`

```
/* 函数原型 */
extern char *ctime (const time_t *__timer);
```

```
#include <stdio.h>
#include <time.h>

int main(void){
    time_t timer = time(NULL);
    printf("%s\n", ctime(&timer)));
}
```

# 参考资料

- C官方资料: http://en.cppreference.com/w/c/chrono/time_t
- byrsongQQ的博文: http://blog.jobbole.com/108049/

