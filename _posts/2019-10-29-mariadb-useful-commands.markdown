---
layout: post
title: MariaDB useful commands
date: '2019-10-29 21:18:25'
categories: mariadb db mysql
---

### Checking current max connections.

{% highlight sql %}
    show status where `variable_name` = 'Max_used_connections';
{% endhighlight %}

### Checking threads connect

{% highlight sql %}
    show status where `variable_name` = 'Threads_connected';
{% endhighlight %}

### Checking current process

{% highlight sql %}
    show full processlist;
{% endhighlight %}
