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

---
### Troubleshooting ConfigMap 

Generally, there are two types of errors that may occur when applying a ConfigMap:
1. Schema Validation Error
2. BigIP Validation Error

#### Schema Validation Error
This type of error is detected before the configuration is sent to BIG-IP.
You can view the error details in the CIS Pod log.

How to Simulate a Schema Validation Error
- Backup the existing ConfigMap.

- Edit the ConfigMap and introduce an intentional error — for example, by adding a , (comma) character after a closing } brace in one or some of the lines.

- Apply the invalid ConfigMap

- Check error in Pod log

#### Best Practice for Schema Validation

- Please refer to

- Before apply the configuration , it is suggested to validate the schema in VS Code

- Copy all lines under word Template to this block in VS Code
  
  ```
  {
    "$schema": "https://raw.githubusercontent.com/F5Networks/f5-appsvcs-extension/refs/heads/main/schema/latest/as3-schema-3.54.0-6.json",
    ...
    ...
  }
  ```
- Select View -> Problems to see schema validation error
  


