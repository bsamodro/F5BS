## CIS - OpenShift Workshop - OCP Cluster Preparation Guide

#### This document explains how to prepare the OpenShift cluster on LAB for first-time use and subsequent reboots. This includes ensuring all nodes are in the `Ready` state and verifying cluster availability.

---

### Prepare Working Directory to run next steps


#### Use the Web Shell link of the ocp-provisioner node and Please keep this window open until the end of the lab

<img width="454" alt="Image" src="https://github.com/user-attachments/assets/93ce4bbe-80b8-40f5-89da-3de378a97e3e" />

- Login as cloud-user
  ```
  su - cloud-user
  ```
- Clean previous firefox docker
  ```
  docker stop firefox
  docker rm firefox
  ```
- Create new firefox docker
  ```bash
  
  docker run -d \
  --name=firefox \
  -p 5800:5800 \
  -v ~/firefox-saas:/config:rw \
  --restart=always \
  --add-host arcadia.f5demo:10.1.10.101 \
  jlesage/firefox
  ```

Use the FIREFOX link of the ocp-provisioner node. With this you will be running a docker firefox remotely inside your browser, from there you can browse to the OpenShift UI URLs (or any other). 

<img width="392" alt="Image" src="https://github.com/user-attachments/assets/a0f94c0d-718c-4baf-86d5-97448fcfb551" />

---

### First Boot or Node is not ready - Approving Node CSRs

In Webshell Terminal, Check if all node status are ready
```bash
oc get nodes
```

Output
```bash
NAME                      STATUS   ROLES                         AGE    VERSION
master-1.ocp.f5-udf.com   Ready    control-plane,master,worker   688d   v1.29.8+f10c92d
master-2.ocp.f5-udf.com   Ready    control-plane,master,worker   688d   v1.29.8+f10c92d
master-3.ocp.f5-udf.com   Ready    control-plane,master,worker   688d   v1.29.8+f10c92d
worker-1.ocp.f5-udf.com   Ready    worker                        688d   v1.29.8+f10c92d
worker-2.ocp.f5-udf.com   Ready    worker                        688d   v1.29.8+f10c92d
```

### If all node are ready , Click following link : [BigIP Preparation](BigIP-Preparation.md)
### If some/all node are not ready , Click following link : [Cluster Recovery](Cluster-Recovery.md)

----

[🏠 Home](readme.md) | [➡️ Next](BigIP-Preparation.md)
