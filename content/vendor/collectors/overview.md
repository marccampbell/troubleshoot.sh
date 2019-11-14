---
date: 2019-10-23
linktitle: "Overview"
title: Collectors Overview
weight: 20010
---

Collectors are YAML specifications that define what to data to collect when generating a support bundle or when running preflight checks on a cluster.

All collectors are specified in a single YAML file. To build a set of collectors, start with a Kubernetes YAML file:

```yaml
apiVersion: troubleshoot.replicated.com/v1beta1
kind: Collector
metadata:
  name: my-application-name
spec: []
```

The above file is a simple but valid support bundle spec. It will collect only the default data.

To add additional collectors, read the docs in this section to understand each one, and add them as an array item below `spec`. Each collector can be included multiple times, if there are different sets of options to use. 

For example, a complete spec might be:

```yaml
apiVersion: troubleshoot.replicated.com/v1beta1
kind: Collector
metadata:
  name: my-application-name
spec:
  - clusterInfo: {}
  - clusterResources: {}
  - logs:
      selector:
        - app=api
      namespace: default
      limits:
        maxAge: 30d
        maxLines: 1000
  - http:
      name: healthz
      get:
        url: http://api:3000/healthz
  - exec:
      name: mysql-version
      selector:
        - app=mysql
      namespace: default
      command: ["mysql"]
      args: ["-V"]
      timeout: 5s
```
