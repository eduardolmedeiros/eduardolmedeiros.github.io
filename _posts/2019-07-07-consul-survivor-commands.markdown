---
layout: post
title: 'Consul: survivals commands'
date: '2019-07-07 12:49:49'
categories: consul
---

Consul is a service mesh solution providing a full featured control plane with service discovery, configuration, and segmentation functionality.

### Listing members

```shell
$ consul members
$ consul members -detailed
$ curl http://consul.your-awesome-domain.com:8500/v1/catalog/nodes | jq
```

### Listing server cluster

```shell
$ consul members | grep -i server
```

### Join cluster

```shell
$ consul join <ip>
```

### Reload

```shell
$ consul reload
```

### Checking health state

```shell
$ curl http://consul.your-awesome-domain.com:8500/v1/health/state/critical | jq
$ curl http://127.0.0.1:8500/v1/operator/autopilot/health | jq
```

###  Listing raft peers

```shell
$ consul operator raft list-peers
```

## Some external utils documentation

#### Backup & Restore

[https://learn.hashicorp.com/consul/datacenter-deploy/backup](https://learn.hashicorp.com/consul/datacenter-deploy/backup)

#### Check list

[https://learn.hashicorp.com/consul/datacenter-deploy/production-checklist](https://learn.hashicorp.com/consul/datacenter-deploy/production-checklist)

#### Outage:

[https://www.consul.io/docs/guides/outage.html](https://www.consul.io/docs/guides/outage.html)

