---
title: JetBrains Quest 2020 解谜挑战
subtitle: JetBrains Quest on Twitter
catalog: true
date: 2020-03-15
tags: ["JetBrains"]
---

# 前言

JetBrains Quest 2020 解谜挑战，解密拿奖励喽，挑战一下吧。

![JetBrains-Quest-2020-1][JetBrains-Quest-2020-1]

> 图：JetBrains Quest 2020

# Round 1

JetBrains 推文上的提示文本明显是倒排过的，需要把文本正排回来。

```plain
pleh A+lrtC/dmC .thgis fo tuo si ti semitemos ,etihw si txet nehw sa drah kooL .tseretni wohs dluohs uoy ecalp a si ,dessecorp si xat hctuD erehw esac ehT .sedih tseuq fo txen eht erehw si ,deificeps era segaugnal cificeps-niamod tcudorp ehT
```
> 注：提示信息

```java
public class Slover {
    public static void main(String[] args) {
        String str = ".spleh A+lrtC/dmC .thgis fo tuo si ti semitemos ,etihw si txet nehw sa drah kooL .tseretni wohs dluohs uoy ecalp a si ,dessecorp si xat hctuD erehw esac ehT .sedih tseuq fo txen eht erehw si ,deificeps era segaugnal cificeps-niamod tcudorp ehT";
        System.out.println(
            new StringBuilder(str).reverse().toString()
        );
    }
}
/* EOF */
```
> 代码清单：Java 正排文本程序

```plain
The product domain-specific languages are specified, is where the next of quest hides. The case where Dutch tax is processed, is a place you should show interest. Look hard as when text is white, sometimes it is out of sight. Cmd/Ctrl+A helps.
```
> 注：解密提示信息 - Round 1

# Round 2

Google 搜索 JetBrains Domain-specific Languages 产品，发现 MPS 。

> JetBrains DSL 产品：https://www.jetbrains.com/mps/

进入页面，提示查看某个客户案例，`Ctrl + F`全文搜索`Dutch tax`关键词，找到对应案例点开。

点开后是一份 PDF 文件，按照提示使用`Ctrl + A`全选反隐，发现隐藏提示信息。

![JetBrains-Quest-2020-2][JetBrains-Quest-2020-2]

> 图：JetBrains Quest 2020 - Round 2 提示信息

```plain
This is our 20th year as a company,
we have shared numbers in our JetBrains
Annual report, sharing the section with
18,650 numbers will progress your quest.
```
> 注：解密提示信息 - Round 2

# Round 3

根据上一轮提示信息，Google 搜索 JetBrains Annual Report 20th，找到 JetBrains 20 周年报告页。

> JetBrains 20 周年报告页：https://www.jetbrains.com/company/annualreport/2019/

观察页面，注意`18,650`数字出现的小节，发现`7th Annual Hackathon`小节出现的所有数字和即为 18650 。点击该小节的分享按钮，找到隐藏提示信息。

![JetBrains-Quest-2020-3][JetBrains-Quest-2020-3]

> 图：JetBrains Quest 2020 - Round 3 提示信息

```java
public class Slover {
    public static void main(String[] args) {
        System.out.println(
            0 + 1 + 2 + 6
          + 7 + 9 + 12 + 56
          + 70 + 182 + 305 + 18000
        );
    }
}
/* EOF */
```
> 代码清单：计算数字和 18,650

```plain
I have found the JetBrains Quest! Sometimes you just need to look closely at the Haskell language, Hello,World! in the hackathon lego brainstorms project https://blog.jetbrains.com/blog/2019/11/22/jetbrains-7th-annual-hackathon/ #JetBrainsQuest
```
> 注：解密提示信息 - Round 3

# Round 4

根据上一轮提示信息，进入 JetBrains 7 周年 Hackathon 比赛页。

> JetBrains 7 周年 Hackathon 比赛页：https://blog.jetbrains.com/blog/2019/11/22/jetbrains-7th-annual-hackathon/

页面定位到 Lego BrainStorms 项目，可以发现一张关于 Haskell 程序设计语言`Hello,World!`图片，图片中有一段文字，查看页面 HTML 源码可以将该文本内容拷贝出来。

![JetBrains-Quest-2020-4][JetBrains-Quest-2020-4]

> 图：JetBrains Quest 2020 - Round 4 提示信息

复制出来的文本，看似乱码，实际上是一段英文的诙谐写法，转译回来，就拿到了这一轮的隐藏提示信息。

```plain
d1D j00 kN0w J378r41n2 12 4lW4Y2 H1R1N9? ch3CK 0u7 73h K4r33r2 P493 4nD 533 1f 7H3r3 12 4 J08 F0r J00 0R 4 KW357 cH4LL3n93 70 90 fUr7h3r @ l3457.
->
Did you know Jetbrains is always hiring? Check out the careers page and see if there is a job for you or for quest challenge to go further at least.
```
> 注：解密提示信息 - Round 4

# Round 5

根据上一轮提示信息，提示我们寻找 JetBrains 招聘页面，搜索找到后进入。

> JetBrains 招聘页：https://www.jetbrains.com/careers/jobs/

这一轮的隐藏提示信息藏在了 JetBrains 招聘岗位之中，发现一枚奇怪的招聘职位，进入查看详情。

![JetBrains-Quest-2020-5][JetBrains-Quest-2020-5]

> 图：JetBrains Quest 2020 - Round 5 提示信息

岗位介绍中包含了这一轮的隐藏提示信息。

```plain
To progress with your quest what you’ll need:
- To check out what we have for game developers.
- Be geeky enough to remember how you used to cheat at Konami games.
- Try cheating on the page.
```
> 注：提示信息 - Round 5

# Round 6

根据上一轮的到的提示信息，提示我们寻找 JetBrains 为游戏开发者准备的内容，搜索之。

> JetBrains 游戏开发者页面：https://www.jetbrains.com/gamedev/

这网页设计得真炫酷！

![JetBrains-Quest-2020-6-1][JetBrains-Quest-2020-6-1]

> 图：JetBrains Game Developers

然后，这个 Konami Game Cheats ，一开始没反应过来，搜索之...

大名鼎鼎的**科乐美秘技**，一代人的游戏回忆，小时候谁没玩过魂斗罗呢，JetBrains 情怀满满。

![JetBrains-Quest-2020-6-2][JetBrains-Quest-2020-6-2]

> 图：科乐美秘技（Konami Code）

在 JetBrains 游戏开发者页面输入科乐美秘技`↑ ↑ ↓ ↓ ← → ← → B A`，弹出来一个打砖块小游戏，把砖块打掉之后，最终奖励就出现啦。

![JetBrains-Quest-2020-6-3][JetBrains-Quest-2020-6-3]

> 图：打砖块小游戏

# Final Reward

前往 JetBrains 奖励认领页面，输入个人邮箱与认证代码，即可领取最终奖励：JetBrains 全家桶 3 个月。

> JetBrains 奖励认领页：http://jb.gg/quest

![JetBrains-Quest-2020-7][JetBrains-Quest-2020-7]

> 图：JetBrains Quest Final Reward

至此，JetBrains Quest 解谜挑战完美结束！

[JetBrains-Quest-2020-1]: ./JetBrains-Quest-2020-1.png

[JetBrains-Quest-2020-2]: ./JetBrains-Quest-2020-2.png

[JetBrains-Quest-2020-3]: ./JetBrains-Quest-2020-3.png

[JetBrains-Quest-2020-4]: ./JetBrains-Quest-2020-4.png

[JetBrains-Quest-2020-5]: ./JetBrains-Quest-2020-5.png

[JetBrains-Quest-2020-6-1]: ./JetBrains-Quest-2020-6-1.png

[JetBrains-Quest-2020-6-2]: ./JetBrains-Quest-2020-6-2.jpg

[JetBrains-Quest-2020-6-3]: ./JetBrains-Quest-2020-6-3.png

[JetBrains-Quest-2020-7]: ./JetBrains-Quest-2020-7.png

<!-- EOF -->
