### Devstinaiton-k8s-staging-environment
#####################################
### Deploy K8s Cluster, Add k8s Kubeconfig file to Github Actions secrets. ###  
# Create devstinaiton namespace
```bash
kubectl create ns devstinaiton
```
# deploy cert-manager 
```bash
kubectl  apply -f ./cert-manager/cert-manager.yaml
```
# deploy metrics-server
```bash
kubectl  apply -f ./metrics-server/metrics-server-deployment.yaml
```
# Create Traefik namespace  
```bash
kubectl apply -f ./traefik/traefik-namespace.yaml
```
# Installing Resource Definition and RBAC  
# Install Traefik Resource Definitions:
```bash
kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v2.10/docs/content/reference/dynamic-configuration/kubernetes-crd-definition-v1.yml
```
# Install RBAC for Traefik:
```bash
kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v2.10/docs/content/reference/dynamic-configuration/kubernetes-crd-rbac.yml
```
# Upgrading CRDs  
```bash
kubectl apply --server-side --force-conflicts -k https://github.com/traefik/traefik-helm-chart/traefik/crds/
```
# Create required PVCs  
```bash
kubectl apply -f ./traefik/traefik-pv-pvc.yaml
```
# Create Traefik Deployment and Service
```bash
kubectl  apply -f ./traefik/traefik-deployment.yaml
```  
# Create ElasticSearch Single Instance and Kibana Deployment and Service.
# Consider ElasticSearch Cluster for Production/Large-scale environment.
```bash
kubectl apply -f ./ElasticSearch-Kibana-SingleInsatnce/pvc.yaml
kubectl apply -f ./ElasticSearch-Kibana-SingleInsatnce/statefulSet.yaml
kubectl apply -f ./ElasticSearch-Kibana-SingleInsatnce/kibana.yaml
```
--------------------------------------------
### Deploy devstinaiton BE/FE services ###
# Github actions configured to trigger/auto-deploy devstinaiton, devstinaitonnodejs and devstinaiton-new-front services to k8s cluster
# development branch >> k8s-staging-cluster
# main branch >> k8s-production-cluster
# Github actions jobs to deploy k8s-staging environment
https://github.com/Devstinaiton/devstinaiton/blob/development/.github/workflows/staging-ci.yml  
https://github.com/Devstinaiton/devstinaitonnodejs/blob/development/.github/workflows/staging-ci.yml  
https://github.com/Devstinaiton/devstinaiton-new-front/blob/development/.github/workflows/staging-ci.yml  

--------------------------------------------
### Deploy devstinaiton Admin and Admin-BE services ###
# Github actions configured to trigger/auto-deploy Admin and Admin-BE services to k8s cluster

https://github.com/Devstinaiton/devstinaiton-admin-backend/blob/development/.github/workflows/staging-ci.yml  
https://github.com/Devstinaiton/devstinaiton-admin/blob/development/.github/workflows/staging-ci.yml  

# Create required devstinaiton IngressRoute
```bash
kubectl  -n devstinaiton apply -f ./traefik/ingressroute.yaml  
```

# TODO
Configured k8s autoscaling for each service HPA.  