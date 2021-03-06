---
layout: post
title: Recording ansible playbooks results with ARA
date: '2021-03-06 14:00:00'
categories: ansible
---

### Introduction

Ansible is awesome and we all know about that, however, sometimes is really complicated to manage a large number of machines without tooling for debugging and analysis of results.

ARA is not a replacement for AWX/Tower, ARA is only used for reporting and could be used with other toolings like AWX/Tower/Rundeck for instance.

### Installation

ARA supports multiples installation method (package, pip, docker, podman), in this example we're going to use docker.

### Docker method

```shell
$ docker run --name api-server --detach --tty --volume ~/.ara/server:/opt/ara:z -p 8000:8000 quay.io/recordsansible/ara-api:latest
```

ARA server is ready for use

### How to prepare ansible side

```shell
# Install Ansible and ARA (without API server dependencies) for the current user
python3 -m pip install --user ansible ara

# Configure Ansible to use the ARA callback plugin
export ANSIBLE_CALLBACK_PLUGINS="$(python3 -m ara.setup.callback_plugins)"

# Set up the ARA callback to know where the API server is located
export ARA_API_CLIENT="http"
export ARA_API_SERVER="http://127.0.0.1:8000"

```

#### Create a simple playbook for test

```shell
cat <<EOF >>playbook.yml
---
- hosts: all
  tasks:
    - name: this is simple ping task
      ping:
EOF
```

#### Executing the playbook

```shell
$ ansible-playbook -c local -i localhost, playbook.yml
```

### Checking results on ARA

#### Use the CLI to see recorded playbooks

```shell
$ ara playbook list
```

```
+----+-----------+--------------------------------------------------------------------------+-------------------------------------------------------+-------+---------+-------+-----------------------------+-----------------+
| id | status    | controller                                                               | path                                                  | tasks | results | hosts | started                     | duration        |
+----+-----------+--------------------------------------------------------------------------+-------------------------------------------------------+-------+---------+-------+-----------------------------+-----------------+
|  1 | completed | Eduardos-MacBook-Pro.local                                               | /Users/medeiros/Downloads/playbook.yml                |     2 |       2 |     1 | 2021-03-06T12:02:34.265387Z | 00:00:02.163620 |
+----+-----------+--------------------------------------------------------------------------+-------------------------------------------------------+-------+---------+-------+-----------------------------+-----------------+
```

#### Checking throught web console

http://localhost:8080

![ARA console](/assets/ara.png)


That's all.