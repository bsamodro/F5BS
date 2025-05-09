
### Lab Diagram


---
### Step and Procedure for handson



---
### Environment

| Hostname           | HostIP     | Access  | Username | Password     |
|--------------------|------------|---------|----------|--------------|
| RKE2Master-1       | 10.1.1.11  | Web Shell / SSH | ubuntu   | ubuntu      |
| Ubuntu Client      | 10.1.1.14  | firefox | <No Password> | <No Password> |
| bigip1.f5demo.id   | 10.1.1.5   | TMUI    | admin    | f5demo#1     |
| bigip2.f5demo.udf  | 10.1.1.15  | TMUI    | admin    | f5demo#1     |


- TMOS Version : 17.1
- RKE2 Version : 1.31.6
- CIS Version : 2.19.1
- AS3 Version : 3.54.0

---

### Reference

Latest IngressLink Depeloyment Guide
- https://clouddocs.f5.com/containers/latest/userguide/ingresslink/
- https://docs.nginx.com/nginx-ingress-controller/installation/integrations/f5-ingresslink/

Latest NGINX IC Manifest Deployment Guide
- https://docs.nginx.com/nginx-ingress-controller/installation/installing-nic/installation-with-manifests/

latest CIS release notes and compatibility with AS3 : 
- https://clouddocs.f5.com/containers/latest/reference/release-notes.html
- https://clouddocs.f5.com/containers/latest/userguide/what-is.html#container-ingress-service-compatibility

---
