---
layout: post
title: Ansible introduction
date: '2019-01-05 18:50:18'
categories: ansible
---

Ansible is a radically simple IT automation engine that automates cloud provisioning, configuration management, application deployment, intra-service orchestration, and many other IT needs.

### Highlights

- Human readable and simple to understand
- Agent less
- Access is done with SSH
- Large support community
- Many roles and modules available
- Host inventory handles and defines the infrastructure
- Free and Open Source Software (FOSS)

### Ansible core components

#### Controller Machine

The machine where Ansible is installed, responsible for running the provisioning on the servers you are managing.InventoryAn initialization file that contains information about the servers you are managing.

#### Playbook

The entry point for Ansible provisioning, where the automation is defined through tasks using YAML format.

#### Task

A block that defines a single procedure to be executed, e.g. Install a package.

#### Module

A module typically abstracts a system task, like dealing with packages or creating and changing files. Ansible has a multitude of built-in modules, but you can also create custom ones.

#### Role

A pre-defined way for organizing playbooks and other files in order to facilitate sharing and reusing portions of a provisioning.

#### Facts

Global variables containing information about the system, like network interfaces or operating system.

#### Handlers

Used to trigger service status changes, like restarting or stopping a service.
