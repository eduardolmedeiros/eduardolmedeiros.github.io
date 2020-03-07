---
layout: post
title: MariaDB useful commands
date: '2019-10-29 21:18:25'
categories: mariadb db mysql
---

### Checking current max connections.

```sql
show status where `variable_name` = 'Max_used_connections';
```

### Checking threads connect

```sql
show status where `variable_name` = 'Threads_connected';
```

### Checking current process

```sql
show full processlist;
```
