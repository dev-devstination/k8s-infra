# Create Traefik namespace  
kubectl apply -f ./traefik/traefik-namespace.yaml

# Installing Resource Definition and RBAC  

# Install Traefik Resource Definitions:
kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v2.10/docs/content/reference/dynamic-configuration/kubernetes-crd-definition-v1.yml

# Install RBAC for Traefik:
kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v2.10/docs/content/reference/dynamic-configuration/kubernetes-crd-rbac.yml

# Upgrading CRDs  
kubectl apply --server-side --force-conflicts -k https://github.com/traefik/traefik-helm-chart/traefik/crds/  

# Create required PVCs  
kubectl apply- f ./traefik/traefik-pv-pvc.yaml

# Create Traefil Deployment and Service
kubectl apply -f ./traefik/traefik-deployment.yaml

# Create required kotal IngressRoute
kubectl apply -f ./traefik/ingressroute.yaml
