---
layout: post
title: 'Molecule: How to use (part 2)'
date: '2019-09-20 14:10:55'
categories: ansible molecule automation
---

On this post, we gonna coverage some useful commands for molecule.

>[!WARNING]
>These settings are based on molecule version 2, it might be changed on version 3.

### Converge will execute the sequence necessary to converge the instances.

```shell
$ molecule converge
```

### This action has cleanup and is not enabled by default. See the provisioner’s documentation for further details.

```shell
$ molecule cleanup
```

### Destroy box/container.

```shell
$ molecule destroy
```

### Runs the converge step a second time. If no tasks will be marked as changed the scenario will be considered idempotent.

```shell
$ molecule idempotence
```

### Initialize a new role.

```shell
$ molecule init role --role-name foo
```

### Execute lint tests

```shell
$ molecule lint
```

### login into the box/container

```shell
$ molecule login
```

### Check syntax on the default scenario.

```shell
$ molecule syntax
```

### Test will execute the sequence necessary to test the instances.

```shell
$ molecule test
```
