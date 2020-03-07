---
layout: post
title: Ansible Ad-Hoc commands
date: '2019-01-05 19:36:47'
categories: ansible
---

> Ad-hoc commands in Ansible allow you to execute simple tasks at the command line against one or all of your hosts.

An ad-hoc command consists of two parameters: the **host group** that defines on what machines to run the task against and the Ansible **module** to run.

These parameters are passed to the ansible binary for invocation.

# Running ad-hoc commands

### ping all hosts

{% highlight shell %}
ansible all -m ping
{% endhighlight %}

### execute the command "uptime" on all hosts

{% highlight shell %}
ansible all -m shell -a 'uptime'
{% endhighlight %}

### run the command disk usage using the inventory json file on all hosts

{% highlight shell %}
ansible all -i inventory.json -m shell -a 'df -h'
{% endhighlight %}

### install the package git as root user on the target host using the inventory.yml file

{% highlight shell %}
ansible test-db-001.mydomain.com -i inventory.yml -b -m package -a "name=git state=present"
{% endhighlight %}
