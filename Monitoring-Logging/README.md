### Devstinaiton-k8s-staging-environment
#####################################
### Deploy K8s Cluster, Add k8s Kubeconfig file to Github Actions secrets. ###  
# Create devstinaiton namespace
```bash
kubectl create ns loki
```

# import grafana
```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```


# deploy loki stack
```bash
 helm install loki grafana/loki-stack --namespace=loki --set grafana.enabled=true,config.table_manager.retention_deletes_enabled=true,config.table_manager.retention_period=360h,prometheus.enabled=true,prometheus.alertmanager.persistentVolume.enabled=false,prometheus.server.persistentVolume.enabled=true,kube-state-metrics.podSecurityPolicy.enabled=false -f loki-stack-values.yaml
```

# Import Required Grafa Dashboards From : 
```bash
https://grafana.com/grafana/dashboards/?dataSource=prometheus
```