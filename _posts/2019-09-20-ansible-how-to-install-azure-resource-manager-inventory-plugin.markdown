---
layout: post
title: 'Ansible: How-to install Azure Resource Manager inventory plugin (part 1)'
date: '2019-09-20 15:27:53'
categories: ansible inventory azure_rm microsoft azure
---

### Introduction

This plugin allows you to work with dynamic inventory on Azure.

### Features  

- Support VMSS
- Conditional groups based on the name of the instances.
- Key group tags creation
- Multiples authentication methods way

### Installation

### Install azure cli
[https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)  
  
### Install azure inventory plugin

{% highlight shell %}
pip install 'ansible[azure]'
{% endhighlight %}

### Setup

Create a new file called azure_rm.yml

{% highlight yaml %}
---
plugin: azure_rm
auth_source: auto
 
# Include VMSS.
include_vmss_resource_groups:
  - '*'
{% endhighlight %}

### Setup Azure subscription settings

**Azure cli**

{% highlight shell %}
az login
az account set --subscription <subscription_name>
{% endhighlight %}

### Invoking ansible cli.

**Listing all hosts**

{% highlight shell %}
ansible-inventory -i azure_rm.yml --list all
{% endhighlight %}

**Pinging**

{% highlight shell %}
ansible all -m ping -i azure_rm.yml -u <ansible_user> --key-file "<ansible_user_ssh_key_file>"
{% endhighlight %}

If you did not get any error, it's working properly.

Next post, I will give some ideas how to improve the inventory.

