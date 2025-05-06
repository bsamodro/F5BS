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
  Output (no needd to copy)
  ```
  NAME                          READY   STATUS    RESTARTS   AGE
  cis-bigip1-5cf4f8d9bc-xlwts   1/1     Running   0          3h3m
  cis-bigip2-855455999b-rxvgw   1/1     Running   0          3h3m
  ```
- check latest log from CIS Pod (see pod name from above command)
  ```bash
  oc logs cis-bigip1-5cf4f8d9bc-xlwts -n kube-system | tail -10
  ```

---
### Troubleshooting ConfigMap 

Generally, there are two types of errors that may occur when applying a ConfigMap:
1. Schema Validation Error
2. NonExist Object Validation Error

---
#### Schema Validation Error
In Certain Case, This type of error is detected before the configuration is sent to BIG-IP.
You can view the error details in the CIS Pod log.

How to Simulate a Schema Validation Error
- Backup the existing ConfigMap.
  ```bash
  cp Arcadia/arcadia-cm.yaml Arcadia/arcadia-cm.yaml_backup
  ```
- Edit the ConfigMap and introduce an intentional error — for example, by adding a , (comma) character after a closing } brace in one or some of the lines.
  ```bash
  vi Arcadia/arcadia-cm.yaml
  ```
- Apply the invalid ConfigMap
  ```
  oc delete -f Arcadia/arcadia-cm.yaml
  oc create -f Arcadia/arcadia-cm.yaml
  ```
- Check error in Pod log
  ```
  oc logs cis-bigip1-5cf4f8d9bc-xlwts -n kube-system | tail -3
  ```
  Output (no needd to copy)
  ```
  2025/05/05 12:38:50 [ERROR] [2025-05-05 12:38:50,298 __main__ ERROR] Error applying config, will try again in 1 seconds
  2025/05/05 12:44:04 [ERROR] Error processing configmap arcadia-cm in namespace: arcadia with err: invalid character ',' looking for beginning of object key string
  2025/05/05 12:44:29 [ERROR] Error processing configmap arcadia-cm in namespace: arcadia with err: invalid character ',' looking for beginning of object key string
  ```
- Rollback to backup config and apply
  ```
  cp Arcadia/arcadia-cm.yaml_backup Arcadia/arcadia-cm.yaml
  oc delete -f Arcadia/arcadia-cm.yaml
  oc create -f Arcadia/arcadia-cm.yaml
  ```
  
#### Schema Validation Best Practice

Before applying the configuration, it's strongly recommended to validate the schema locally using Visual Studio Code (VS Code).

📘 Reference:
👉 https://clouddocs.f5.com/products/extensions/f5-appsvcs-extension/latest/userguide/validate.html

Steps to Validate in VS Code
1. Open VS Code.
2. Copy all lines under the Template section from your configmap configuration : arcadia-cm.yaml
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

---
#### NonExist Object Validation Error

This type of error typically occurs due to a typo in the object name. For example, the policy name might be correctly configured as /Common/arcadia-waf, but a mistake such as typing /Common/arcadia_waf (underscore instead of hyphen) will result in an error because BIG-IP cannot find the specified object.

You can view detailed error information in the CIS Pod logs. In most cases, BIG-IP will respond with an HTTP status code 422 (Unprocessable Entity) when this happens.

How to Simulate NonExist Object Validation Error

- Edit the ConfigMap and change waf policy from arcadia-waf to arcadia_waf
  ```
  vi Arcadia/arcadia-cm.yaml
  ```
- Apply the invalid ConfigMap
  ```
  oc delete -f Arcadia/arcadia-cm.yaml
  oc create -f Arcadia/arcadia-cm.yaml
  ```
- Check error in Pod log
  ```
  oc logs cis-bigip1-5cf4f8d9bc-xlwts -n kube-system | tail -3
  ```
  ```
  2025/05/05 12:48:50 [ERROR] [2025-05-05 12:48:50,319 __main__ ERROR] Failed to process the config file /tmp/k8s-bigip-ctlr.config1337228655/config.json (json.decoder.JSONDecodeError: Expecting value: line 1 column 1 (char 0))
  2025/05/05 12:48:50 [ERROR] [2025-05-05 12:48:50,320 __main__ ERROR] Error applying config, will try again in 1 seconds
  2025/05/05 12:59:22 [ERROR] [AS3] Big-IP Responded with error code: 422
  ```
- Rollback to backup config and apply
  ```
  cp Arcadia/arcadia-cm.yaml_backup Arcadia/arcadia-cm.yaml
  oc delete -f Arcadia/arcadia-cm.yaml
  oc create -f Arcadia/arcadia-cm.yaml
  ```

You can also check this type of error in the BIG-IP logs for further details.

- Use the SSH link of bigip1
- Check restnode log
  ```bash
  tail -3 /var/log/restnoded/restnoded.log
  ```
  ```
  Mon, 05 May 2025 13:05:29 GMT - finest: socket 2623 closed
  Mon, 05 May 2025 13:05:54 GMT - finest: socket 2624 opened
  Mon, 05 May 2025 13:05:56 GMT - warning: [appsvcs] {"message":"unable to digest declaration. Error: Unable to find specified WAF policy /Common/arcadia_waf for /arcadia-tenant/arcadia-app/arcadia-VS/policyWAF","level":"warning"}
  ```

---


