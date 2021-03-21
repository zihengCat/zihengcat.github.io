---
title: 为 Airflow 加装 Cryptography 库
subtitle: Using Cryptography Package in Airflow
catalog: true
date: 2018-08-26
tags: [Linux, Python, Apache, Airflow]
---

# 前言

`Apache Airflow`分布式任务调度框架默认将密码等敏感信息以**明文形式**存储到元数据（Metadata）库中，这会带来一定的安全隐患。Airbnb 官方也强烈建议开发者安装`cryptography`库以实现敏感信息的加密存储。

另外，如果不安装`cryptography`加密库，Airflow 的许多高级功能都是无法使用的。

# 为`Airflow`加装`cryptography`库

Apache Airflow 默认不会安装`cryptography`库，我们需要手动安装并配置。

首先，使用`pip`命令来安装上`cryptography`库。

```bash
$ pip install apache-airflow[crypto]
```
> 代码清单：`pip`安装`apache-airflow[crypto]`

第二步，使用`Python`代码生成一枚`fernet_key`。`fernet_key`的格式必须为`base64-encoded`，长度必须为`32-bytes`。

```bash
$ python -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())"
...
```
> 代码清单：`Python`生成`fernet_key`

然后，配置 Airflow 使用`fernet_key`。将第二步得到的**秘钥**写入到**配置文件`airflow.cfg`**中，也可以额外设置一枚环境变量。

```bash
$ vim airflow.cfg
...
fernet_key = <fernet_key>
...
```
> 代码清单：写`airflow.cfg`配置文件

```bash
$ export AIRFLOW__CORE__FERNET_KEY='<fernet_key>'
```
> 代码清单：设置`AIRFLOW__CORE__FERNET_KEY`环境变量

最后，重启 Airflow 服务「`scheduler`，`webserver`」即可。

# 参考资料

- `Apache Airflow`官方文档：https://airflow.apache.org/howto/secure-connections.html

- 为`Airflow`生成`fernet_key`：https://bcb.github.io/airflow/fernet-key

