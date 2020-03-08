---
layout: post
title: Rundeck: managing the api
date: '2020-03-08 01:15:00'
categories: rundeck api
---

# rundeck api

Rundeck is an open-source automation service with a web console and command-line tools.

In addition, what is nice about [Rundeck](http://www.rundeck.org) is that you are able to manage it via API, which is super nice for integration with other systems eg: Jenkins and so.

Follow some examples about how to use the Rundeck API.

## list projects
```shell
curl -k -X "GET" -H "Accept: application/json" -H "X-Rundeck-Auth-Token: YOUR-TOKEN-HERE" https://your-rundeck-server.com/api/26/projects | jq `
```

## list jobs inside projects
```shell
curl -k -X "GET" -H "Accept: application/json" -H "X-Rundeck-Auth-Token: YOUR-TOKEN-HERE" https://your-rundeck-server.com/api/26/project/[project-name]/jobs | jq .
```

## execute a job
```shell
curl -k -X "POST" -H "Accept: application/json" -H "X-Rundeck-Auth-Token: YOUR-TOKEN-HERE" https://your-rundeck-server.com/api/26/job/[job-id]/run | jq .
```

## get a job result
```shell
curl -k -X "GET" -H "Accept: application/json" -H "X-Rundeck-Auth-Token: YOUR-TOKEN-HERE" https://your-rundeck-server.com/api/26/execution/[job-execution-id] | jq ".status"
```

## run specific host

Host level:

```shell
curl -k -X "POST" -H "Accept: application/json" -H "X-Rundeck-Auth-Token: YOUR-TOKEN-HERE" --data-urlencode "filter=[host]" https://your-rundeck-server.com/api/26/job/[job-id]/run | jq .
```

Tags (for this example test environment)

```shell
curl -k -X "POST" -H "Accept: application/json" -H "X-Rundeck-Auth-Token: YOUR-TOKEN-HERE" --data-urlencode "filter=tags:[environment]" https://your-rundeck-server.com/api/26/job/[job-id]/run | jq .
```