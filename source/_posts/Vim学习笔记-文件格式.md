---
title: Vim学习笔记 - 文件格式
catalog: true
date: 2017-09-04
tags: Vim
---

# 断行符的故事

很久以前，老式的电传打字机使用**两枚字符**来另起新行。一枚字符把滑动架移回首位，称为*回车（Carriage Return）*，另一枚字符把纸上移一行，称为*换行（Line Feed）*。

当计算机问世以后，存储器曾经一度非常昂贵。有些人就认定，没有必要使用两枚字符来表示行尾。UNIX 的开发者们决定，他们可以只用`<LF>`一个字符来表示行尾。而 Apple 的开发者们则决定只使用`<CR>`。开发 MS-DOS（以及后来的 Microsoft Windows）的那些家伙们则决定沿用老式的`<CR><LF>`。

这意味着，如果你试图把一个文件从一种操作系统转移到另一种操作系统，可能会遇到「换行符」方面的麻烦。

> 注：摘录自`vimdoc`

下表展示了不同操作系统下文本文件断行字符的区别。

| UNIX   | MS-DOS    | Macintosh |
| :----: | :-------: | :-------: |
| `<LF>` | `<CR><LF>`| `<CR>`    |
| `\n`   | `\r\n`    | `\r`      |
| `0x0A` | `0x0D0A`  | `0x0D`    |

> 表：不同操作系统断行符区别
> 注：Mac OS X 后，Macintosh 系统断行符与 UNIX 相同了

# 查看转换断行字符

Vim文本编辑器能自动识别不同文件格式，我们可以非常方便地实现不同操作系统之间的文件格式转换。

Vim中，我们可以使用`set: f[ile]f[ormat]`命令查看当前编辑文本的文本格式。vim使用`unix`，`dos`，`mac`标识不同系统的文本格式。

```
:set ff?
```
> 代码清单：命令查看当前编辑文本的文本格式

不同系统默认设置不同。比如在Linux系统下，vim优先识别与使用UNIX断行符。

```
fileformats=unix,dos        /* Unix based systems */
fileformats=dos,unix        /* Windows and DOS systems */
fileformats=mac,unix,dos    /* Mac OS 9 systems */
```
> 表：不同系统下断行符选择优先级

在Vim中完成转换文本格式非常简单，只需设置一下当前编辑文本的`fileformat`选项即可。vim会自动帮助我们完成断行字符的转换工作。

```
:set ff=dos
```
> 代码清单：vim 下转换 UNIX 文本格式至 MS-DOS 文本格式

# 参考资料

- vim 帮助文档：`:help ff`，`:help file-formats`
- vim 维基百科：http://vim.wikia.com/wiki/File_format

