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
[🏠 Home](readme.md) | [➡️ Next](BigIP-Preparation.md)
