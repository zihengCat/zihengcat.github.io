---
title: Microsoft Excel 会计账目自动化核对
date: 2017-12-06
tags: Tips
---

# 前言

昨天，有位会计系的同学拜托子恒喵，希望我能帮他解决一个关于**会计账目核对**的问题，这是他兼职淘宝店会计时碰到的实际问题。问题不难，解决问题后，子恒喵觉得这个实例挺有代表性，值得记录一下。

# 问题描述

现有淘宝女装店铺某月销售记录表，退换货记录表(两张Excel表格)。
每一条退换货记录都有一条销售记录与之对应，但是由于各种原因，会有**退换货记录与销售记录不匹配**的情况(客户名，商品名，商品价格不匹配)。
需要找出这些错误记录，修改正确。

# 问题解决

最开始的时候啊，会计系同学将两张表合在一起，排序后(销售纪录与退换货记录紧挨)手工核对。
可是，销售记录表有*20多万*条数据，退换货记录*2万多*条数据，手工核对...看一会儿眼就花了。
子恒喵使用Excel自带的VBA开发者工具实现账目核对自动化。
打开Excel开发者工具VBA，撰写如下代码:
```
Sub data_check()
    '循环变量
    Dim i As Long
    Dim j As Long

    '获取两表长度
    rows_1 = Sheets("退换货记录表").usedrange.rows.count
    rows_2 = Sheets("销售记录表").usedrange.rows.count

    For i = 2 To rows_1
      For j = 2 To rows_2
        '匹配三项数据
        If Sheets("销售记录表").Cells(i, 1) =
           Sheets("退换货记录表").Cells(i, 1) Then
            If Sheets("销售记录表").Cells(i, 3) =
               Sheets("退换货记录表").Cells(i, 3) Then
                If Sheets("销售记录表").Cells(i, 5) =
                   Sheets("退换货记录表").Cells(i, 5) Then
                    '如果匹配成功则标注出来
                     Sheets("退换货记录表").Cells(i, 10) = "匹配"
                End If
            End If
        End If
      Next j
    Next i
End Sub
```
通过这段VBA代码，我们就可以方便的实现账目核对自动化。
