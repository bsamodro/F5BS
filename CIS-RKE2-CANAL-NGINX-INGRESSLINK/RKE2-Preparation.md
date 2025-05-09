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
  vi deployments/common/nginx-config.yaml #
  ```
  Apend these line and clode vi editor [ Esc :wq ]
  ```
    proxy-protocol: "True"
    real-ip-header: "proxy_protocol"
    set-real-ip-from: "0.0.0.0/0"
  ```

  - Deploy config map and ingress clas
  ```
  kubectl apply -f deployments/common/nginx-config.yaml
  kubectl apply -f deployments/common/ingress-class.yaml
  ```

  - Create core custom resources
  ```
  kubectl apply -f config/crd/bases/k8s.nginx.org_virtualservers.yaml
  kubectl apply -f config/crd/bases/k8s.nginx.org_virtualserverroutes.yaml
  kubectl apply -f config/crd/bases/k8s.nginx.org_transportservers.yaml
  kubectl apply -f config/crd/bases/k8s.nginx.org_policies.yaml
  kubectl apply -f config/crd/bases/k8s.nginx.org_globalconfigurations.yaml
  ```

  - # Update  NGINX Ingress Controller - Using a DaemonSet
  ```
  cp deployments/daemon-set/nginx-ingress.yaml deployments/daemon-set/nginx-ingress.yaml_ori
  vi deployments/daemon-set/nginx-ingress.yaml 
  ```
  # Before (replace this line, no need to copy)
  ```
          args:
          - -nginx-configmaps=$(POD_NAMESPACE)/nginx-config
          - -report-ingress-status
          - -external-service=nginx-ingress
  ```
  # Change with this then close vi editor
  ```
          args:
          - -nginx-configmaps=$(POD_NAMESPACE)/nginx-config
          - -default-server-tls-secret=$(POD_NAMESPACE)/default-server-secret
          - -ingresslink=nginx-ingress
          - -report-ingress-status
         # - -external-service=nginx-ingress
  ```

  - Deploy NGINX Ingress Controller
  ```
  kubectl apply -f deployments/daemon-set/nginx-ingress.yaml
  ```

  - Create Cluster IP NGINX Ingress Service
  ```
  vi deployments/service/nginx-service.yaml
  ```
  # Insert this line
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: nginx-ingress-ingresslink
    namespace: nginx-ingress
    labels:
      app: nginx-ingress
  spec:
    ports:
      - port: 80
        targetPort: 80
        protocol: TCP
        name: http
      - port: 443
        targetPort: 443
        protocol: TCP
        name: https
    selector:
      app: nginx-ingress
    type: ClusterIP
  
