## CIS - OpenShift Workshop - CIS Troubleshooting Guide

#### This document explains how to do troubleshooting CIS on OCP Cluster
---

### Troubleshooting CIS in OCP Cluster

When deploying CIS on OpenShift (OCP), CIS will fully control the BIG-IP configuration. Therefore, if there are issues during service deployment, the first log to check is the CIS log in OCP.

It is not recommended to set the log level to DEBUG, as it generates excessive logs, making it harder to identify the actual error. The INFO level is generally sufficient to capture relevant error messages.

- Find CIS Pod
  ```bash
  oc get pods -n kube-system
  ```
      NAME                          READY   STATUS    RESTARTS   AGE
      cis-bigip1-5cf4f8d9bc-xlwts   1/1     Running   0          3h3m
      cis-bigip2-855455999b-rxvgw   1/1     Running   0          3h3m

- check latest log from CIS Pod (see pod name from above command)
  ```bash
  oc logs cis-bigip1-5cf4f8d9bc-xlwts -n kube-system | tail -10
  ```
