# k8s-monitoring
### create rbac/svc/pv
```
helm reset --force
kubectl apply -f role-binding.yaml
kubectl apply -f rbac-config.yaml
helm init --service-account tiller --wait
```
### create local directories in all nodes 
```
mkdir -p /opt/k8s-volumes/prometheus-alertmanager
mkdir -p /opt/k8s-volumes/prometheus-server
mkdir -p /opt/k8s-volumes/grafana
```

### deploy prometheus
```
helm repo update
helm install stable/prometheus --namespace monitoring --name prometheus```
#### edit pvc and deploy pv
   `cd ./k8s-monitoing/`
1- `kubectl edit pvc prometheus-alertmanager -n monitoing`
 - add `volumeName: prometheus-alertmanager`
2- `kubectl edit pvc prometheus-server -n monitoing`
 - add `volumeName: prometheus-server`
3- `kubectl apply -f ./prometheus/pv.yml`
4- `kubectl apply -f ./prometheus/pv-man.yml`
5- `kubectl apply -f ./gragana/pv.yml`
#### edit svc to be node-port
kubectl delete svc grafana -n monitoring
kubectl apply -f ./grafana/service.yml -n monitoring
### deploy garafana
`cd charts/stable/grafana`
`helm install stable/grafana --set persistence.enabled=true --set persistence.accessModes={ReadWriteOnce} --set persistence.size=8Gi -n grafana --namespace monitoring`


- get admin password 
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
#### edit svc to be nodePort

check this [URL](https://youtu.be/tYIqsby5gBc)
