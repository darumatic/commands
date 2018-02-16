# Pre-requisites
* Kubernetes 1.7.0+
* Helm 2.6.0+

# Installation
```bash
helm repo add coreos https://s3-eu-west-1.amazonaws.com/coreos-charts/stable/
helm install coreos/prometheus-operator --name prometheus-operator --namespace monitoring --set server.service.type=NodePort
helm install coreos/kube-prometheus --name kube-prometheus --set global.rbacEnable=true --set global.service.type=NodePort --set prometheus.service.type=NodePort --namespace monitoring
```

# Accessing the services 
```bash
kubectl edit svc -n monitoring kube-prometheus-grafana  

#change
#  type: ClusterIP
#for:
#  type: NodePOrt

#Same change below:
kubectl edit svc -n monitoring kube-prometheus-alertmanager
```

## Accesing the services  

```bash
kubectl get svc -n monitoring 
```
Get the service port for: 
* Main Prometheus service: kube-prometheus-prometheus
* Graphan: kube-prometheus-grafana
* Prometheus Alert Manager kube-prometheus-alertmanager


# Notes

* Nevermind the DeadMansSwitch alert. Is just a dummy alert to verify the system is working fine.
* Inside prometheus alerts, pay attention to the runbook links, since it contains recommendations into how to fix the alerts. Example: https://imgur.com/a/9VFNV

# References and / or further information
* https://itnext.io/kubernetes-monitoring-with-prometheus-in-15-minutes-8e54d1de2e13
* https://github.com/coreos/prometheus-operator/tree/master/helm
* https://github.com/prometheus/alertmanager
* https://prometheus.io/docs/alerting/configuration/
* https://prometheus.io/webtools/alerting/routing-tree-editor/
* https://hub.kubeapps.com/charts/stable/prometheus
