---
layout: post
title: 'Molecule: Enabling roles dependencies'
date: '2019-09-20 14:39:45'
categories: ansible molecule automation
---

Molecule supports roles dependencies, however, we need to define the role-file parameter into the molecule.yml file to fetch the roles automatically.

1) Change the **molecule.yml** file according to the sample below.

{% highlight yaml %}
dependency:
  name: galaxy
  options:
    role-file: ${MOLECULE_SCENARIO_DIRECTORY}/requirements.yml
{% endhighlight %}

2) Create a file called **requirements.yml** within molecule folder.

{% highlight yaml %}
---
# sample for springboot role.
- src: git@gitlab.yourdomain.com:/ansible/roles/springboot.git
  scm: git
{% endhighlight %}


3) Add the role into the **playbook.yml** file.

{% highlight yaml %}
- name: Converge
  hosts: all
  roles:
    - role: springboot
    - role: myrole
{% endhighlight %}
