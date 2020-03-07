---
layout: post
title: 'Molecule: How to install (part 1)'
date: '2019-09-17 19:36:51'
categories: ansible automation molecule
---

> Molecule is designed to aid in the development and testing of Ansible roles.

> Molecule provides support for testing with multiple instances, operating systems and distributions, virtualization providers, test frameworks and testing scenarios.

> Molecule encourages an approach that results in consistently developed roles that are well-written, easily understood and maintained.

There are several ways to install molecule, for instance using package manager like yum, apt and brew, however we gonna coverage only one method which is pip.

Since python 2 is almost end-of-life, I recommend to install molecule using python3, but of course I will show how to install using python (which is basically the same procedure).

Molecule requires docker and python installed.

### python 3

Use pip to install the virtualenv Python module:This will ensure that your package repository includes the latest version of the python-pip package, which will install pip and Python 3.7.

We will use pip to create a virtual environment and install additional packages.

<!--kg-card-begin: markdown-->

    python3 -m pip install virtualenv

<!--kg-card-end: markdown-->

Next, let's create and activate the virtual environment:

<!--kg-card-begin: markdown-->

    python3 -m virtualenv molecule

<!--kg-card-end: markdown-->

Activate it to ensure that your actions are restricted to that environment:

<!--kg-card-begin: markdown-->

    source molecule/bin/activate

<!--kg-card-end: markdown-->

Install molecule and docker using pip:

<!--kg-card-begin: markdown-->

    python3 -m pip install molecule docker

<!--kg-card-end: markdown-->
### python 2

Use pip to install the virtualenv Python module. This will ensure that your package repository includes the latest version of the python-pip package, which will install pip and Python 2.7.

We will use pip to create a virtual environment and install additional packages.

<!--kg-card-begin: markdown-->

    python -m pip install virtualenv

<!--kg-card-end: markdown-->

Next, let's create and activate the virtual environment:

<!--kg-card-begin: markdown-->

    python -m virtualenv molecule

<!--kg-card-end: markdown-->

Activate it to ensure that your actions are restricted to that environment:

<!--kg-card-begin: markdown-->

    source molecule/bin/activate

<!--kg-card-end: markdown-->

Install molecule and docker using pip:

<!--kg-card-begin: markdown-->

    python -m pip install molecule docker

<!--kg-card-end: markdown-->

Next post I will explain how molecule works.

