---
title: Linux下unzip乱码解决
date: 2017-07-21
tags: [Linux, Tips]
---

# 问题

`zip`文档是 Windows 与 Linux 下常见的压缩文档格式。因其开源，易用的特点而广为流行。我们在 Linux 下使用`unzip`命令解压缩 Windows 系统压缩的 zip 文档时，时常出现**文件名乱码**的问题。

# 原因

由于 zip 并没有指定编码格式，在 Windows 下生成的 zip 文件中的编码一般都是 GBK/GB2312 等。而 Linux 下的默认编码是 UTF-8，编码格式不匹配，所以解压缩时就会乱码。

# 解决

实际上，`unzip`命令带有指定字符编码这一选项。
```
$ unzip --help
...
-O CHARSET  specify a character encoding for DOS, Windows and OS/2 archives
-I CHARSET  specify a character encoding for UNIX and other archives
...
```
我们只需在`unzip`解压文件时候加上`-O`选项指定字符集即可。
```
$ unzip -O CP936 <filename.zip>
```

> 微软的 CP936 通常被视为等同 GBK，连 IANA 也以“CP936”为“GBK”之别名。事实上比较起来，GBK 定义之字符较 CP936 多出95字（15个非汉字及80个汉字），
> > ---- 维基百科

