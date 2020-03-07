---
layout: post
title: 'Ansible: Azure Resource Manager inventory plugin (part 2)'
date: '2019-10-03 21:20:06'
categories: ansible azure_rm azure 
---

Continuing the ARM inventory plugin post. Follow below some useful settings to help you to generate a good dynamic inventory.

### Working with group conditional

```yaml
conditional_groups:
  # since this will be true for every host, every host sourced from this inventory plugin config will be in the
  # group 'all'
  all: true
  # if the VM's "name" variable contains "test", it will be placed in the 'test' group.
  test: "'test' in name"
  # if the VM's "name" variable contains "dev", it will be placed in the 'preprod' group.
  preprod: "'preprod' in name"
  # if the VM's "name" variable contains "prod", it will be placed in the 'prod' group.
  prod: "'prod' in name"
```

### Working with tag conditional

```yaml
# places hosts in dynamically-created groups based on a variable value.
keyed_groups:
# places each host in a group named 'tag_(tag name)_(tag value)' for each tag on a VM.
- prefix: "azure_loc"
  key: "location"
- prefix: "flavor"
  key: tags.flavor | default('none')
```

### Fetches VMs from an explicit list of resource groups instead of default all

```yaml
include_vm_resource_groups:
- myrg1
- myrg2
```

### Disabling instances in shutoff state.

```yaml
# excludes hosts that are powered off
- powerstate != 'running'
```

### Excluding hosts

```yaml
# excludes hosts in the eastus region
- location in ['east']
```
