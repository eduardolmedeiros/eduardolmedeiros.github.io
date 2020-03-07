---
layout: post
title: 'MariaDB: How-to check connections and threads connected'
date: '2019-10-01 22:24:52'
categories: mariadb sql mysql
---

Sometimes we have to do some troubleshooting inside our database environment such as checking the current connections and threads for instance. The number of connections and threads are good indicators for capacity planning as well. In this post, I will show some commands to get that information.

### Getting the current settings:

```shell
MariaDB [(none)]> show global variables like 'max_connections';
+-----------------+-------+
| Variable_name | Value |
+-----------------+-------+
| max_connections | 200 |
+-----------------+-------+
1 row in set (0.00 sec)
```

```shell
MariaDB [(none)]> show global variables like 'thread_pool_max_threads';
+-------------------------+-------+
| Variable_name | Value |
+-------------------------+-------+
| thread_pool_max_threads | 200 |
+-------------------------+-------+
1 row in set (0.00 sec)
```

### Checking the current status:

```shell
show status where `variable_name` = 'Max_used_connections';
show status where `variable_name` = 'Threads_connected';
```
