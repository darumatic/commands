# Pre-requisites
* Kubernetes 1.7.0+
* Helm 2.6.0+

# Installation
```bash
helm repo add coreos https://s3-eu-west-1.amazonaws.com/coreos-charts/stable/
helm install coreos/prometheus-operator --name prometheus-operator --namespace monitoring
helm install coreos/kube-prometheus --name kube-prometheus --set global.rbacEnable=true --namespace monitoring
```

# Accessing the services locally
```bash
kubectl port-forward -n monitoring prometheus-kube-prometheus-0 9090
kubectl port-forward $(kubectl get  pods --selector=app=kube-prometheus-grafana -n  monitoring --output=jsonpath="{.items..metadata.name}") -n monitoring  3000
kubectl port-forward -n monitoring alertmanager-kube-prometheus-0 9093
```
Now we can access: 
* Main Prometheus service in: http://localhost:9090/  
* Graphana: http://localhost:3000/  
* Prometheus Alert Manager: http://localhost:9093/  

# Notes

* Nevermind the DeadMansSwitch alert. Is just a dummy alert to verify the system is working fine.
* Inside prometheus alerts, pay attention to the runbook links, since it contains recommendations into how to fix the alerts. 

# References and / or further information
* https://itnext.io/kubernetes-monitoring-with-prometheus-in-15-minutes-8e54d1de2e13
* https://hub.kubeapps.com/charts/stable/prometheus
* https://github.com/prometheus/alertmanager
* https://prometheus.io/docs/alerting/configuration/
* https://prometheus.io/webtools/alerting/routing-tree-editor/
