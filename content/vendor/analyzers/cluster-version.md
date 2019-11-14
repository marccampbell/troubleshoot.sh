---
date: 2019-11-01
linktitle: "Cluster Version"
title: Cluster Version
weight: 20020
---

The clusterVersion analyzer is used to report on the installed version of Kubernetes. This checks the server/cluster version, not the version of kubectl. The `when` attribute specifies a semver range to compare the running version against and supports all standard comparison operators.

To implement an analyzer that has a minimum version, specify that version as a fail or warn outcome first, and have a default outcome for pass. This will allow the pass outcome to always succeed when the fail or warn outcomes are not truthy.

An example clusterVersion analyzer that reports a failure on Kubernetes less than 1.14.0, a warning when running 1.14.x, and a pass on 1.15.0 or later is:

```yaml
apiVersion: troubleshoot.replicated.com/v1beta1
kind: Preflight
metadata:
  name: check-kubernetes-version
spec:
  analyzers:
    - clusterVersion:
        outcomes:
          - fail:
              when: "< 1.14.0"
              message: The application requires at Kubernetes 1.14.0 or later, and recommends 1.15.0.
              uri: https://kubernetes.io
          - warn:
              when: "< 1.15.0"
              message: Your cluster meets the minimum version of Kubernetes, but we recommend you update to 1.15.0 or later.
              uri: https://kubernetes.io
          - pass:
              message: Your cluster meets the recommended and required versions of Kubernetes.
```
