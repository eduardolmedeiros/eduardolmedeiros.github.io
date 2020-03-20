---
layout: post
title: 'Ansible: Best practices checker with pre-commit and ansible-lint'
date: '2020-03-20 22:00:00'
categories: ansible pre-commit
---

## Introduction

### Git hook

Git hook scripts are useful for identifying simple issues before submission to code review. We run our hooks on every commit to automatically point out issues in code such as missing semicolons, trailing whitespace, and debug statements. By pointing these issues out before code review, this allows a code reviewer to focus on the architecture of a change while not wasting time with trivial style nitpicks.

### Pre-commit project

[Pre-commit](https://pre-commit.com/) is a framework for managing and maintaining multi-language pre-commit hooks.

### Ansible lint

[ansible-lint](https://github.com/ansible/ansible-lint) it's a tool that checks playbooks for practices and behavior that could potentially be improved.


Let's install pre-commit with ansible-lint support.


## Install pre-commit

```
pip install pre-commit
```

### Enabling ansible-lint pre-commit

1.  Create a pre-commit configuration file:

`.pre-commit-config.yaml`

```yaml
---
repos:
- repo: https://github.com/ansible/ansible-lint.git
  rev: v4.2.0
  hooks:
    - id: ansible-lint
      files: \.(yaml|yml)$
```

2. Enable pre-commit for your git repository

```
$ pre-commit install
pre-commit installed at .git/hooks/pre-commit
```


## Testing pre-commit

```
$ pre-commit run --all-files
Ansible-lint.............................................................Passed
```

or just type:

```
$ git commit 
```

Great! right?

Mean's my ansible project is fine and no errors found.

Now let's see a sample for error on ansible side:

```
git commit -m "testing hook" -a
Ansible-lint.............................................................Failed
- hook id: ansible-lint
- exit code: 2

[701] Role info should contain license
meta/main.yml:2
{'meta/main.yml': {'skipped_rules': [], '__file__': u'/Users/medeiros/src/github/eduardolmedeiros/ansible-dummy-role/meta/main.yml', u'dependencies': [], u'galaxy_info': {u'description': u'dummie ansible role', u'author': u'Eduardo Medeiros', u'company': u'emedeiros.me', u'galaxy_tags': [], '__line__': 3, u'platforms': [{'__file__': u'/Users/medeiros/src/github/eduardolmedeiros/ansible-dummy-role/meta/main.yml', u'name': u'EL', '__line__': 32, u'versions': [7, 8]}], '__file__': u'/Users/medeiros/src/github/eduardolmedeiros/ansible-dummy-role/meta/main.yml', u'min_ansible_version': 2.9}, '__line__': 2}}
```

Well. That's all. Using pre-commit you can avoid some broken codes at production and at the same time making your teammates happy.

Cheers!
