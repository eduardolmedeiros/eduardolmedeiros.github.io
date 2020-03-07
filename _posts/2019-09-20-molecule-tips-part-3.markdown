---
layout: post
title: 'Molecule: How to create a new role and change some default settings.'
date: '2019-09-20 14:20:15'
categories: ansible automation molecule
---

## Creating new roles

### Role with docker driver support

    molecule init role --role-name myrole --driver-name docker 

### Role with vagrant driver support

    molecule init role --role-name myrole --driver-name vagrant 

## Molecule settings

### Docker (molecule.yml)

Sample file with systemd suppport and port 8080 exposed.

```yaml
    ---
    dependency:
      name: galaxy
    driver:
      name: docker
    lint:
      name: yamllint
    platforms:
      - name: motown2-proxy.molecule.local
        image: centos:7
        privileged: true
        volume_mounts:
          - "/sys/fs/cgroup:/sys/fs/cgroup:rw"
        command: "/usr/sbin/init"
        privileged: true
        pre_build_image: true
        exposed_ports:
          - 8080/tcp
        published_ports:
          - 0.0.0.0:8080:8080/tcp
        dns_servers:
          - 8.8.8.8
          - 8.8.4.4
    provisioner:
      name: ansible
      lint:
        name: ansible-lint
    scenario:
      name: default
    verifier:
      name: testinfra
      env:
        PYTHONWARNINGS: "ignore:.*U.*mode is deprecated:DeprecationWarning"
      lint:
        name: flake8
```

### Vagrant (molecule.yml)

Sample file for Vagrant with port 8080 exposed.

```yaml
    ---
    dependency:
      name: galaxy
    driver:
      name: vagrant
      provider:
        name: virtualbox
    lint:
      name: yamllint
    platforms:
      - name: instance
        box: centos/7
        instance_raw_config_args:
          - "vm.network 'forwarded_port', guest: 8080, host: 8080"
        memory: 1024
        cpus: 1
    provisioner:
      name: ansible
      lint:
        name: ansible-lint
    scenario:
      name: default
    verifier:
      name: testinfra
      env:
        PYTHONWARNINGS: "ignore:.*U.*mode is deprecated:DeprecationWarning"
      lint:
        name: flake8
```