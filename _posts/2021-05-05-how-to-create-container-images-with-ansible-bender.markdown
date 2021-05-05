---
layout: post
title: How to create container images with ansible-bender
date: '2021-05-05 14:15:00'
categories: ansible
---

### Introduction

[Ansible Bender](https://github.com/ansible-community/ansible-bender) is a tool that gives the ability to creates containers using Ansible playbooks and turns them into container images.
Ansible-bender relies on Ansible connection plugins (buildah) for performing builds.


### Features

* You can build your container images with buildah as a backend.
* Ansible playbook is your build recipe.
* You are able to set various image metadata via CLI or as specific Ansible vars:
    * working directory
    * environment variables
    * labels
    * user
    * default command
    * exposed ports
    * volume mounts during the build


### Why should you care?

* Ansible can take care of your container images
* Using Ansible to define an image instead of a bash script
* You can recycle your playbooks and roles
* Ansible playbooks are YAML and easy to maintain


### Requirements

I'm using Fedora 34 for this setup, if you are using another distro, please have a look at install documentation.

#### Podman

Podman is a tool for managing containers and images, volumes mounted into those containers, and pods made from groups of containers. 

![Podman](/assets/podman-logo.png)

```shell
$ dnf install podman
```
[Install doc](https://podman.io/getting-started/installation)

#### Buildah

Buildah - a tool that facilitates building Open Container Initiative (OCI) container images.
more details: https://opencontainers.org/

![Buildah](/assets/buildah-logo.png)

```shell
$ dnf install buildah
```
[Install doc](https://github.com/containers/buildah/blob/master/install.md)


### Installing Ansible Bender

Option 1. Easy way - Downstream packages (stable versions)

```shell
$ dnf install ansible-bender
```

Option 2. Setup using python pip (upstream packages/latest versions)

```shell
$ python3 -m virtualenv .env
$ .env/bin/pip install --upgrade pip
$ .env/bin/pip install ansible selinux ansible-bender
$ source .env/bin/activate
```

>In my case, I will take the option 2, because I would like to try out the latest version.

### Let's try out

In this example, I'll create a simple image with [azcopy](https://github.com/Azure/azure-storage-azcopy) (Azure Storage data transfer utility) tooling installed.

#### 1. Lets create a playbook.yml file

```yaml
---
- hosts: all
  vars:
    ansible_bender:
      base_image: "docker.io/library/python:3-alpine"
      target_image:
        name: "docker.io/emedeiros/azcopy:{{ azcopy_version }}"
        labels:
          version:  {% raw %}"{{ azcopy_version }}"{% endraw %}
        cmd: "{{ azcopy_bin_path }}/azcopy"
    azcopy_pkg_url: https://azcopyvnext.azureedge.net/release20210415/azcopy_linux_amd64_10.10.0.tar.gz
    azcopy_bin_path: /usr/local/bin
    azcopy_version: 10.10.0

  tasks:
    - name: Install utils
      package:
        name: "{{ item }}"
        state: present
      loop:
        - unzip
        - tar
        - libc6-compat
      
    - name: "Install azcopy {{ azcopy_version }} package"
      unarchive:
        remote_src: true
        src: "{{ azcopy_pkg_url }}"
        dest: "{{ azcopy_bin_path }}"
        extra_opts: [--strip-components=1]

    - name: Register current version installed
      command: "{{ azcopy_bin_path }}/azcopy --version"
      register: "version"    

    - name: Check installation
      fail:
        msg: "azcopy {{ azcopy_version }} installation failure"
      when: version.stdout.find(azcopy_version) == -1

    - name: Display version installed
      debug:
        msg: "{{ version.stdout_lines }}"
```       

#### 2. Build a container image on top of that.

```shell
ansible-bender build ./playbook.yml --no-cache

PLAY [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont] ***********

TASK [Gathering Facts] *********************************************************
ok: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont]

TASK [Install utils] ***********************************************************
changed: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont] => (item=unzip)
changed: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont] => (item=tar)
changed: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont] => (item=libc6-compat)

TASK [Install azcopy 10.10.0 package] ******************************************
changed: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont]

TASK [Register current version installed] **************************************
changed: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont]

TASK [Check installation] ******************************************************
skipping: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont]

TASK [Display version installed] ***********************************************
ok: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont] => {
    "msg": [
        "azcopy version 10.10.0"
    ]
}

PLAY RECAP *********************************************************************
docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont : ok=5    changed=3    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   

Getting image source signatures
Copying blob sha256:b2d5eeeaba3a22b9b8aa97261957974a6bd65274ebd43e1d81d0a7b8b752b116
Copying blob sha256:ff362123de14f499ce9b7bb1907ff5955b1938f615a28e197b1481421c5c73e7
Copying blob sha256:cf9eca8251cd62ceb3a229ca035194b95a08a4dc365b39f9ef7ebf642ded304b
Copying blob sha256:20b112836d68cd0ab33d97dba32b52802b47033c5010fff39c1e900ee296d3c1
Copying blob sha256:dcdc49916e794252075ff5dd1f365e201b54c0184914a7e72378654a8d728b29
Copying blob sha256:092fe46bf30114b055a176ba559eeebbc16d7749824a260fbe73eb2168981dad
Copying config sha256:73155863de9e6907f0daa82ea28f87963847e9cb0794377a0e462a952622a9b1
Writing manifest to image destination
Storing signatures
73155863de9e6907f0daa82ea28f87963847e9cb0794377a0e462a952622a9b1
Image 'docker.io/emedeiros/azcopy:10.10.0' was built successfully \o/
```


#### 3. Check the logs 

```shell
$ ansible-bender list-builds
```

```shell
$ ansible-bender get-logs [ID]

PLAY [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont] ***********

TASK [Gathering Facts] *********************************************************
ok: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont]

TASK [Install utils] ***********************************************************
changed: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont] => (item=unzip)
changed: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont] => (item=tar)
changed: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont] => (item=libc6-compat)

TASK [Install azcopy 10.10.0 package] ******************************************
changed: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont]

TASK [Register current version installed] **************************************
changed: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont]

TASK [Check installation] ******************************************************
skipping: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont]

TASK [Display version installed] ***********************************************
ok: [docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont] => {
    "msg": [
        "azcopy version 10.10.0"
    ]
}

PLAY RECAP *********************************************************************
docker-io-emedeiros-azcopy-10-10-0-20210505-124806108269-cont : ok=5    changed=3    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0    
```

#### 4. Inspecting the image

```shell
$ ansible-bender inspect 
```

```json
ansible_extra_args: null
base_image: docker.io/library/python:3-alpine
build_container: docker-io-emedeiros-azcopy-10-10-0-20210504-004735245186-cont
build_finished_time: 20210504-004802331000
build_id: '54'
build_start_time: 20210504-004738906160
build_user: null
build_volumes: []
buildah_from_extra_args: null
builder_name: buildah
cache_tasks: false
debug: false
final_layer_id: f9d6847614df9cc52d0363e47036678a391b423cea0adf086758020048f7c2a6
layering: true
layers:
- base_image_id: null
  cached: true
  content: null
  layer_id: 81fc860d6a56caaca274cfca7731c5b6a70462aa4a682bae01fac1c8b755915f
- base_image_id: 81fc860d6a56caaca274cfca7731c5b6a70462aa4a682bae01fac1c8b755915f
  cached: false
  content: 231c2edbda7f1dc0873ddb80e2cd01bca729a6daf16858688c3a8cdd3c0c30aa2d62a9f82a0842431c160fa5e618b7441db3251ef1ae5dc20e3ec8b3b511a6e0
  layer_id: sha256:c2375d7e2a61a414eae19a44e4f104f6f8ad8553d4e455b2b2eb74ae7b02f22d
- base_image_id: sha256:c2375d7e2a61a414eae19a44e4f104f6f8ad8553d4e455b2b2eb74ae7b02f22d
  cached: false
  content: 231c2edbda7f1dc0873ddb80e2cd01bca729a6daf16858688c3a8cdd3c0c30aa2d62a9f82a0842431c160fa5e618b7441db3251ef1ae5dc20e3ec8b3b511a6e0
  layer_id: sha256:745661e4a80aeaae998490b4e35338e2b7e012c1df00da6cb4b25cc2356cbb72
- base_image_id: sha256:745661e4a80aeaae998490b4e35338e2b7e012c1df00da6cb4b25cc2356cbb72
  cached: false
  content: 231c2edbda7f1dc0873ddb80e2cd01bca729a6daf16858688c3a8cdd3c0c30aa2d62a9f82a0842431c160fa5e618b7441db3251ef1ae5dc20e3ec8b3b511a6e0
  layer_id: sha256:a7e8af07e7b26140b7899dd75c2b1eb4bbd3f8be5516d97e5e0ee5ccefa869dc
- base_image_id: sha256:a7e8af07e7b26140b7899dd75c2b1eb4bbd3f8be5516d97e5e0ee5ccefa869dc
  cached: false
  content: 231c2edbda7f1dc0873ddb80e2cd01bca729a6daf16858688c3a8cdd3c0c30aa2d62a9f82a0842431c160fa5e618b7441db3251ef1ae5dc20e3ec8b3b511a6e0
  layer_id: sha256:a2c8876b1a89a37118b9fde9c21109e274e0c32186a9245a0a7170e0c8fcbb7c
- base_image_id: sha256:a2c8876b1a89a37118b9fde9c21109e274e0c32186a9245a0a7170e0c8fcbb7c
  cached: false
  content: 1614e2ee75928f45566e53ef469c231015ea35658c7eced751a936d08188f17e763d7c3698d0836e40a7ba9d5f2bd989637b7c83bd9da6db6b110809c6d964f2
  layer_id: sha256:20638d7872905e67c89bb16a089927c95f4a8122fceab33a89c4bef399db0f40
- base_image_id: sha256:20638d7872905e67c89bb16a089927c95f4a8122fceab33a89c4bef399db0f40
  cached: false
  content: fb0262070dba94482b9af356d55d87005019009007a7ac89c7c9067875c055284dadfe7aba37a0d6bc7b5848fd1767a1c294f2bfe33768a27dac1929fc188862
  layer_id: sha256:39e77550d3b2035b807cbbb682b41f2a9298c80ac669301439d32f599f57624d
- base_image_id: sha256:39e77550d3b2035b807cbbb682b41f2a9298c80ac669301439d32f599f57624d
  cached: false
  content: 430e9e45296402e1efa2330bcdf58adfa24faabdd3e7be6b3e30e604dbebbc840f149693e129619db196d4673234bf67512d93d27e19aade3e8ac26db6107a8e
  layer_id: sha256:a095cfc4562fafdb4ff321fcc826f362ae123a6e46ee39de3d0396c91ce883fd
metadata:
  annotations: {}
  cmd: /usr/local/bin/azcopy
  entrypoint: null
  env_vars: {}
  labels:
    version: 10.10.0
  ports: []
  user: null
  volumes: []
  working_dir: null
playbook_path: ./playbook.yml
pulled: false
python_interpreter: null
squash: false
state: done
target_image: docker.io/emedeiros/azcopy:10.10.0
verbose: false
verbose_layer_names: null
```

#### 5. Testing the image

```shell
$ podman run emedeiros/azcopy:10.10.0
````

output expected:

```shell
docker run emedeiros/azcopy:10.10.0
AzCopy 10.10.0
Project URL: github.com/Azure/azure-storage-azcopy

AzCopy is a command line tool that moves data into and out of Azure Storage.
To report issues or to learn more about the tool, go to github.com/Azure/azure-storage-azcopy

The general format of the commands is: 'azcopy [command] [arguments] --[flag-name]=[flag-value]'.

Usage:
  azcopy [command]
...
```

#### 6. Push your image to your repo (optional)

```shell
$ podman login docker.io
Login Succeeded!
$ podman push docker.io/emedeiros/azcopy:10.10.0 
```

### Bonus 

#### Why not ansible-roles?

```shell
ansible-galaxy role install eduardolmedeiros.azcopy -p roles
```

playbook sample:

```yaml
---
- hosts: all
  vars:
    ansible_bender:
      base_image: "docker.io/library/centos:8"
      target_image:
        name: "docker.io/emedeiros/azcopy"

    - name: import role azcopy
      include_role:
        name: eduardolmedeiros.azcopy
```

That's all folks.
Cheers.