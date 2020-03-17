---
layout: post
title: Cookiecutter for Ansible playbooks
date: '2020-03-17 18:00:00'
categories: ansible cookiecutter
---

## Introduction

I do hate doing repetitive tasks, for instance creating playbooks scaffolds and so one.

Well, I've started my journey looking for some tool that provides such a way to create project templates, then I've found [Cookiecutter](https://cookiecutter.readthedocs.io/) project.

Serious. This project is awesome and you should have a look at that, especially if you are trying to standardized your projects across different teams inside your organization.

Let's have a look at my ansible-playbook cookiecutter template.

## Cookiecutter installation

```shell
$ pip install cookiecutter
```

## Generate a new Cookiecutter template layout

```shell
$ cookiecutter gh:eduardolmedeiros/cookiecutter-ansible-playbook
```

The command above gonna pull the source files from my [github repository](https://github.com/eduardolmedeiros/cookiecutter-ansible-playbook) and generate the output files.

### Wizard menu

After running the cookiecutter command, a wizard menu will popup.
This information came from `cookiecutter.json` file.

```shell
$ cookiecutter gh:eduardolmedeiros/cookiecutter-ansible-playbook
full_name [Your name]: Eduardo Medeiros
email [Your address email (eq. you@example.com)]: eduardo@dotmac.com.br
project_name [Name of the project]: myproject
project_slug [myproject]:
project_short_description [A short description of the project]: This is my first cookiecutter project
release_date [2020-03-17]:
version [0.1.0]:
vagrant_hostname [myproject.vagrant.local]:
vagrant_cpu [2]:
vagrant_memory [2048]:
vagrant_box [centos/7]:
Select license:
1 - MIT license
2 - BSD license
3 - Apache Software License 2.0
4 - GNU General Public License v3
Choose from 1, 2, 3, 4 [1]: 1
```

### Done! Now let's have a look at output files (project folder)

```shell
├── README.MD
├── group_vars
│   └── all.yml
├── host_vars
│   └── myproject.vagrant.local.yml
├── inventory
│   └── inventory.ini
├── myproject-playbook.yml
├── provisioner
│   └── Vagrantfile
└── roles
    └── requirements.yml
```

## A little bit more about cookiecutter-ansible-playbook

* Ansible Playbook scaffold
* Vagrant support
* Dynamic settings defined by wizard menu


That's all, see you next time.