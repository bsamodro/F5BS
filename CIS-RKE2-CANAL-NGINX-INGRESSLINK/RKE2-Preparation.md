##  RKE2 and NGINX Preparation Guide

#### This document explains how to prepare RKE2 and NGINX 
---

### Prepare Working Directory to run next steps


#### Use the Web Shell link of the RKE2-Master1 node and Please keep this window open until the end of the lab


- Login as ubuntu
  ```bash
  su - ubuntu
  ```
- delete existing prebuilt NGINX RKE2
  ```bash
  helm uninstall rke2-ingress-nginx -n kube-system
  ```
  
- Clone NGINX Repository
  ```bash
  git clone https://github.com/nginx/kubernetes-ingress.git --branch v4.0.1
  cd kubernetes-ingress
  ```

- Create a namespace and a service account:
  ```bash
  kubectl apply -f deployments/common/ns-and-sa.yaml
  ```

- Create a cluster role and binding for the service account
  ```
  kubectl apply -f deployments/rbac/rbac.yaml
  ```

- Create common resources
  ```
  kubectl apply -f examples/shared-examples/default-server-secret/default-server-secret.yaml
  ```

- Update config map wuth 3 lines
  ```
  cp deployments/common/nginx-config.yaml deployments/common/nginx-config.yaml_ori
  ```
  Apend these line and clode vi editor [ Esc :wq ]
  
  ```
    proxy-protocol: "True"
    real-ip-header: "proxy_protocol"
    set-real-ip-from: "0.0.0.0/0"
  ```
