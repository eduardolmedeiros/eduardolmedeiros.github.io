---
layout: post
title: 'Molecule: How to define the vault password file.'
date: '2019-09-20 14:28:34'
categories: ansible molecule automation
---

Sometimes we have to deal with encrypted values, such as passwords and sensitive information inside the role, therefore for that case, we have to define the vault configuration for the molecule.

The configuration for ansible vault key password file is defined within the file `$HOME/.config/molecule/config.yml`

```yaml
provisioner:
name: ansible
config_options:
    defaults:
      vault_password_file: ${HOME}/.ansible/.vault
```
