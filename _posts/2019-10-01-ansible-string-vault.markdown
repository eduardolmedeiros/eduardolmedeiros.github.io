---
layout: post
title: 'Ansible: string vault'
date: '2019-10-01 22:47:43'
categories: ansible vault
---

Ansible Vault is a feature of ansible that allows you to keep sensitive data such as passwords or keys in encrypted files, rather than as plaintext in playbooks or roles. These vault files can then be distributed or placed in source control.

The ansible-vault encrypt\_string command will encrypt and format a provided string   
into a format that can be included in ansible-playbook YAML files.

## Passing vault key file.

```shell
ansible-vault encrypt_string --vault-password-file=.vault --name 'some_easy_label' ‘secret’
```

## Without vault key file.

```shell
ansible-vault encrypt_string --name 'some_easy_label' 'secret'
```

## As input (copy/paste)

```shell
ansible-vault encrypt_string --name 'some_easy_label'
```
