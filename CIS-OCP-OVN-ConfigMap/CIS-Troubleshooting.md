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

#### Schema Validation Best Practice

Before applying the configuration, it's strongly recommended to validate the schema locally using Visual Studio Code (VS Code).

📘 Reference:
👉 https://clouddocs.f5.com/products/extensions/f5-appsvcs-extension/latest/userguide/validate.html

Steps to Validate in VS Code
1. Open VS Code.
2. Copy all lines under the Template section from your AS3 configuration.
<img width="735" alt="Image" src="https://github.com/user-attachments/assets/b9553452-0ab3-4f30-b613-9332deceaaed" />

3. Paste them into a new file or validation block in VS Code (use JSON format).
  
  ```
  {
    "$schema": "https://raw.githubusercontent.com/F5Networks/f5-appsvcs-extension/refs/heads/main/schema/latest/as3-schema-3.54.0-6.json",
    ...
    ...
  }
  ```
4. Select View → Problems to view any schema validation errors.
<img width="1071" alt="Image" src="https://github.com/user-attachments/assets/50237aeb-d589-46df-a0fa-cecd675ac82f" />

5. Fix any issues highlighted before applying the configuration.

Example:
In this sample, there is a typo in the pool monitoring value:
"tcp_half_open" (should be a valid monitor name like http).
VS Code will highlight this as a schema error under the "Problems" tab.


  


