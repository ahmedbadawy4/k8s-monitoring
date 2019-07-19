# k8s-monitoring
`using Grafana and  Prometheus`
`git clone https://github.com/helm/charts.git`
### create rbac/svc/pv


### create local directories in all nodes 
```
mkdir -p /opt/k8s-volumes/prometheus-alertmanager
mkdir -p /opt/k8s-volumes/prometheus-server
mkdir -p /opt/k8s-volumes/grafana
```

### deploy prometheus
```
cd charts/stable/prometheus
helm install --name=prometheus . --namespace monitoring --set rbac.create=true
```

#### edit svc to be node-port

### deploy garafana
`cd charts/stable/grafana`
`helm install stable/grafana --set persistence.enabled=true --set persistence.accessModes={ReadWriteOnce} --set persistence.size=8Gi -n grafana --namespace monitoring`
- get admin password 
```
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
#### edit svc to be nodePort

check this [URL](https://youtu.be/tYIqsby5gBc)
