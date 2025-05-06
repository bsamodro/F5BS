## CIS - OpenShift Workshop - OCP Cluster Preparation Guide

#### This document explains how to prepare the OpenShift cluster on LAB for first-time use and subsequent reboots. This includes ensuring all nodes are in the `Ready` state and verifying cluster availability.

---

### Using the OpenShift UI (Optional)

Use the FIREFOX link of the ocp-provisioner node. With this you will be running a docker firefox remotely inside your browser, from there you can browse to the OpenShift UI URLs (or any other). Run following command in ssh access, if you're unable to access firefox
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
    jlesage/firefox
  ```
---

### First Boot or Node is not ready - Approving Node CSRs

Login to ocp-provisioner : Click access -> click ssh -> Open Terminal

<img width="480" alt="Image" src="https://github.com/user-attachments/assets/f400a813-056a-4da1-bd01-89bc2be09e90" />

Check if all node status are ready
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

### If all node are ready , jump to next step : BigIP Preparation

----

Use this procedure **only on first boot**, or when the cluster fails to bring nodes into `Ready` state.

```bash
oc config use-context default/api-ocp-f5-udf-com:6443/recovery

while date; do
  oc get nodes
  oc get csr --no-headers | grep Pending | awk '{print $1}' | xargs --no-run-if-empty oc adm certificate approve
  sleep 5
done
```
Keep running the loop above until all nodes show Ready in the output of:

```bash
oc get nodes
```

All Node status are ready
```bash
NAME                      STATUS   ROLES                         AGE    VERSION
master-1.ocp.f5-udf.com   Ready    control-plane,master,worker   688d   v1.29.8+f10c92d
master-2.ocp.f5-udf.com   Ready    control-plane,master,worker   688d   v1.29.8+f10c92d
master-3.ocp.f5-udf.com   Ready    control-plane,master,worker   688d   v1.29.8+f10c92d
worker-1.ocp.f5-udf.com   Ready    worker                        688d   v1.29.8+f10c92d
worker-2.ocp.f5-udf.com   Ready    worker                        688d   v1.29.8+f10c92d
```

Wait 5–10 minutes after starting the environment, then log in with:

```bash
oc login -u f5admin -p f5admin
```

Use the following to monitor OpenShift component status:
```bash
watch oc get co
```
All components should show Available=True, Progressing=False, and Degraded=False.

If you’re still unable to log in after 15–20 minutes, or get connection errors:
- Re-run the CSR approval loop from the first boot step.
- Make sure all nodes are in Ready state with oc get nodes.
- Ensure your context is set to the correct recovery context:

```bash
oc config use-context default/api-ocp-f5-udf-com:6443/recovery
```

---


