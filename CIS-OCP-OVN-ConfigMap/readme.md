### Lab Diagram

![Image](https://github.com/user-attachments/assets/6f0176a1-aad2-4ae4-9dc7-96163c219dcb)

---
### Step and Procedure for handson

- Step 1 : Check Reference and Variable
https://github.com/bsamodro/F5BS/tree/main/CIS-OCP-OVN-ConfigMap

- Step 2 : Check OCP Readiness (Probably can be discarded , if the cluster is ready)
  https://github.com/bsamodro/F5BS/blob/main/CIS-OCP-OVN-ConfigMap/OCP-Preparation.md

- Step 3 : Prepare BigIP
  https://github.com/bsamodro/F5BS/blob/main/CIS-OCP-OVN-ConfigMap/BigIP-Preparation.md

- Step 4 : Deploy CIS
  https://github.com/bsamodro/F5BS/blob/main/CIS-OCP-OVN-ConfigMap/CIS-Deployment.md

- Step 5 : Test and Validate Arcadia Apps
  https://github.com/bsamodro/F5BS/blob/main/CIS-OCP-OVN-ConfigMap/Arcadia-ValidationAndTest.md

- Step 6 (optional) : Troubleshoot CIS 
  https://github.com/bsamodro/F5BS/blob/main/CIS-OCP-OVN-ConfigMap/CIS-Troubleshooting.md
---
### Environment

| Hostname           | HostIP     | Access  | Username | Password     |
|--------------------|------------|---------|----------|--------------|
| ocp-provisioner    | 10.1.1.4   | firefox | f5admin  | f5admin      |
| Ubuntu Client      | 10.1.1.14  | firefox | <No Password> | <No Password> |
| bigip1.f5demo.id   | 10.1.1.5   | TMUI    | admin    | f5demo#1     |
| bigip2.f5demo.udf  | 10.1.1.15  | TMUI    | admin    | f5demo#1     |


- TMOS Version : 17.1
- Openshift Version : 4.16.15
- CIS Version : 2.19.1
- AS3 Version : 3.54.0

---

### Reference

Latest CIS OCP Integration Guide :
- https://clouddocs.f5.com/containers/latest/userguide/openshift/

latest CIS release notes and compatibility with AS3 : 
- https://clouddocs.f5.com/containers/latest/reference/release-notes.html
- https://clouddocs.f5.com/containers/latest/userguide/what-is.html#container-ingress-service-compatibility

How to validade AS3 in config map
- https://clouddocs.f5.com/products/extensions/f5-appsvcs-extension/latest/userguide/validate.html
---
