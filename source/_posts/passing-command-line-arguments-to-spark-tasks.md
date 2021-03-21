---
title: 为 Spark 任务传递命令行（CLI）参数
subtitle: Passing Command Line Arguments to Spark Tasks
catalog: true
date: 2018-12-07
tags: ["BigData", "Spark"]
---

# 前言

有时，我们在编写`Spark`任务（Job）时，希望传递命令行参数（Command Line Arguments）进任务脚本（Scrpits）。大多数情况下，我们可以利用指定**环境变量**实现这一目的。实际上，我们仍有更优雅的实现方法。

# 为 Spark 任务传递命令行（CLI）参数

为了传递命令行参数（Command Line Arguments）进`Spark`任务脚本（Scrpits），我们可以自定义一枚`SparkConf`配置项键值对，在`spark-submit`提交任务时通过`--conf`选项传入该键值对。注意，这里的键（Key）可以自由定义，不要求固定为`spark.driver.args`。

```bash
$ spark-submit --conf spark.driver.args='arg1 arg2 arg3' <TARGET_SPARK_SCRIPTS>
...
```
> 代码清单：通过`SparkConf`传递命令行参数

在`Spark`任务脚本中，通过`get()`方法即可获取到目标`SparkConf`键值对。

```scala
val sc = new SparkConf()
val args = sc.getConf.get("spark.driver.args").split("\s+")
val param1 = args(0)
val param2 = args(1)
val param3 = args(2)
println("param1 passed from shell : " + param1)
println("param2 passed from shell : " + param2)
println("param3 passed from shell : " + param3)
System.exit(0)
```
> 代码清单：`Scala`获取`SparkConf`命令行参数

```python
if __name__ == "__main__":
    sc = SparkConf()
    args = sc.getConf().get("spark.driver.args").split("\s+")
    print(args)
```
> 代码清单：`Python`获取`SparkConf`命令行参数

# 参考资料

- StackOverflow: https://stackoverflow.com/questions/29928999/

- Script(s) by Ganesh: https://www.gchandra.com/hadoop/spark/spark-scala-command-line-arguments.html

