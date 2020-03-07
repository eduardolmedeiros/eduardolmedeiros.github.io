---
layout: post
title: 'Molecule: Working with inventory'
date: '2019-09-20 14:58:29'
categories: ansible automation molecule
---

In this post, I will explain how to enable and manage inventory on the molecule.   
This is could be interesting when you have to parse some inventory within the role.  
  
1) Edit the file: molecule/default/playbook.yml

{% highlight yaml %}
    ---
    - name: Converge
      hosts: all
      roles:
        - role: my_role
{% endhighlight %}

2) Add the inventory configuration into the file molecule/default/molecule.yml

{% highlight yaml %}
    ---
    dependency:
      name: galaxy
    driver:
      name: docker
    lint:
      name: yamllint
    platforms:
      - name: instance
        image: centos:7
    provisioner:
      name: ansible
      lint:
        name: ansible-lint
      inventory:
        links:
          hosts: inventory.yml
    verifier:
      name: testinfra
      lint:
        name: flake8
{% endhighlight %}

3) Create molecule/default/inventory.yml file (also support JSON, ini format)

{% highlight yaml %}
    ---
    all:
      children:
        azure:
          children:
            acceptance:
              children:
                backend:
                  children:
                    tomcat:
                      hosts:
                        host1.domain.net:
                        host2.domain.net:
                    jboss:
                      hosts:
                        host3.domain.net:
                        host4.domain.net:
                frontend:
                  children:
                    nginx:
                      hosts:
                        host5.domain.net:
                        host6.domain.net:
                    varnish:
                      hosts:
                        host7.domain.net:
                        host8.domain.net:
{% endhighlight %}
