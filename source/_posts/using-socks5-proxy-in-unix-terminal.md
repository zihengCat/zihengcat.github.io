---
title: 设置 UNIX 终端 Socks5 代理连接
subtitle: Using Socks5 Proxy in UNIX Terminal
catalog: true
date: 2018-02-27
tags: [Linux, Macintosh, Tips]
---

# 前言

终端工具（Terminal）是每一位程序员的好伙伴，但是由于「GFW」的原因，国内恶劣的网络环境让程序员们有时只能看着 CLI 提示信息「连接断开，下载失败...」欲哭无泪。

本文介绍 UNIX 系统下，设置终端网络程序走`socks5`代理连接的方法，适用于 UNIX、Linux、macOS。

# 终端「Terminal」设置

在终端下，设置环境变量`ALL_PROXY`为本地`socks5`代理连接。

```bash
$ export ALL_PROXY=socks5://127.0.0.1:<port>
```
> 代码清单：终端下设置本地`socks5`代理
> 注：`<port>`为本地代理连接端口号。

我们可以通过观察`IP`来判断代理连接是否设置成功。

```bash
$ curl -X GET -i 'http://ip.cn'
```
> 代码清单：使用`curl`观察IP

# 脚本化「Auto Script」设置

我们可以将上述命令写入到`~/.bash_profile`中，实现「脚本化」快捷设置。

```bash
# 设置终端代理
function setProxy() {
    export ALL_PROXY="socks5://127.0.0.1:${1}"
}
# 取消终端代理
function unsetProxy() {
    unset ALL_PROXY
}
# 测试终端代理
function testProxy() {
    curl -i 'http://ip.cn'
}
```
> 代码清单：脚本化快捷设置`socks5`代理
> 注：使用`shell`函数，`alias`别名都可以实现

```
$ setProxy <port>   # 启动终端代理连接
...
$ unsetProxy        # 关闭终端代理连接
```
> 代码清单：快捷调用

# 注意事项

设置环境变量`ALL_PROXY`后，终端下只要**支持此环境变量**的网络应用程序大多都会走这个代理「如：Homebrew」，但不是全部。另外，`socks5`代理会与一些「国内镜像源」发生冲突，这也是需要注意的地方。

# 参考资料

- 掘金：https://juejin.im/entry/5821840cd203090055134cc0

<!-- EOF -->

