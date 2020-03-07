---
layout: post
title: Ansible inventory
date: '2019-01-05 19:02:24'
categories: ansible
---

Inventory is a list of servers that Ansible uses to manage the resources.

There are two different type of inventories:

### Static

Manual inventory that we manage.

### Dynamic

The inventory source is imported automatically from cloud providers as EC2, Azure, OpenStack, and more.

### How to use the inventory file?

There are many ways to setup the inventory file on your controller machine.

### Inventory file location

| level | path | note |
| --- | --- | --- |
| root | /etc/ansible/hosts | |
| user | $USER/.ansible/hosts | recommended |

If you are using a different path, you must to specify the inventory file using the parameter -i.

### Creating a basic inventory file

Ansible support many formats, below we gonna show three (INI,YAML and JSON).

For this example, we gonna create two groups with two hosts inside in each.

{% highlight %}
| group | hosts |
| --- | --- |
| webserver | test-web-001.mydomain.com, test-web-002.mydomain.com |
| database | test-db-001.mydomain.com, test-db-002.mydomain.com |
{% endhighlight %}

### INI format
{% highlight %}
    [webserver]
    test-web-001.mydomain.com
    test-web-002.mydomain.com
     
    [database]
    test-db-001.mydomain.com
    test-db-002.mydomain.com
{% endhighlight %}

### YAML format
{% highlight yaml %}
---
all:
    children:
        webserver:
          hosts:
            test-web-001.mydomain.com:
            test-web-002.mydomain.com:
        database:
          hosts:
            test-db-001.mydomain.com:
            test-db-002.mydomain.com:
{% endhighlight %}

### JSON format
{% highlight json %}
{
    "all": {
        "children": {
            "webserver": {
                "hosts": {
                    "test-web-001.mydomain.com": null,
                    "test-web-002.mydomain.com": null
                }
            },
            "database": {
                "hosts": {
                    "test-db-001.mydomain.com": null,
                    "test-db-002.mydomain.com": null
                }
            }
        }
    }
}
{% endhighlight %}

### Listing the Inventory

{% highlight shell %}
$ ansible-inventory --inventory-file=[inventory_file] --list
{% endhighlight %}

### Listing the Inventory and filter by a specific host

{% highlight shell %}
$ ansible-inventory --inventory-file=[inventory_file] --host [hostname]
{% endhighlight %}