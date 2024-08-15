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
--------------------------------------------
### Deploy devstinaiton BE/FE services ###
# Github actions configured to trigger/auto-deploy devstinaiton, devstinaitonnodejs and devstinaiton-new-front services to k8s cluster
# development branch >> k8s-staging-cluster
# main branch >> k8s-production-cluster
# Github actions jobs to deploy k8s-staging environment
https://github.com/dev-devstination/api/blob/development/.github/workflows/development-ci.yml
https://github.com/dev-devstination/cost-etimation-panel
--------------------------------------------
# Create required devstinaiton IngressRoute
```bash
kubectl  -n devstinaiton apply -f ./traefik/ingressroute.yaml  
```

# TODO
Configured k8s autoscaling for each service HPA.  