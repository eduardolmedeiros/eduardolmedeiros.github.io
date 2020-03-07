---
layout: post
title: 'Molecule: How to use (part 2)'
date: '2019-09-20 14:10:55'
categories: ansible molecule automation
---

On this post, we gonna coverage some useful commands for molecule.

### Converge will execute the sequence necessary to converge the instances.

{% highlight shell %}
$ molecule converge
{% endhighlight %}

### This action has cleanup and is not enabled by default. See the provisioner’s documentation for further details.

{% highlight shell %}
$ molecule cleanup
{% endhighlight %}

### Destroy box/container.

{% highlight shell %}
$ molecule destroy
{% endhighlight %}

### Runs the converge step a second time. If no tasks will be marked as changed the scenario will be considered idempotent.

{% highlight shell %}
$ molecule idempotence
{% endhighlight %}

### Initialize a new role.

{% highlight shell %}
$ molecule init role --role-name foo
{% endhighlight %}

### Execute lint tests

{% highlight shell %}
$ molecule lint
{% endhighlight %}

### login into the box/container

{% highlight shell %}
$ molecule login
{% endhighlight %}

### Check syntax on the default scenario.

{% highlight shell %}
$ molecule syntax
{% endhighlight %}

### Test will execute the sequence necessary to test the instances.

{% highlight shell %}
$ molecule test
{% endhighlight %}
