# Create Docker Registry Secret
```
kubectl create secret docker-registry nexus --docker-server=localhost:5000 --docker-username=admin --docker-password= --docker-email=hello@darumatic.com
```

# Kubernetes components quick health check
```
kubectl get componentstatuses
```
You should see something like:

NAME                 STATUS    MESSAGE              ERROR
controller-manager   Healthy   ok                   
scheduler            Healthy   ok                   
etcd-1               Healthy   {"health": "true"}   
etcd-0               Healthy   {"health": "true"} 

# Memory / resource check
```
for i in `kubectl get nodes |grep -v NAME|cut -d ' ' -f 1`; do kubectl top node $i;done
```
The output will be something like:
NAME              CPU(cores)   CPU%      MEMORY(bytes)   MEMORY%  
u-k8s-master-01   90m          2%        5297Mi          37%      
NAME              CPU(cores)   CPU%      MEMORY(bytes)   MEMORY%  
u-k8s-master-02   109m         2%        8541Mi          59%      
NAME              CPU(cores)   CPU%      MEMORY(bytes)   MEMORY%  
u-k8s-master-03   159m         4%        9265Mi          64%      
NAME              CPU(cores)   CPU%      MEMORY(bytes)   MEMORY%  
u-k8s-worker-01   196m         5%        3564Mi          49%      
NAME              CPU(cores)   CPU%      MEMORY(bytes)   MEMORY%  
u-k8s-worker-02   231m         5%        5955Mi          82%      
NAME              CPU(cores)   CPU%      MEMORY(bytes)   MEMORY%  
u-k8s-worker-03   204m         5%        4844Mi          66%      
NAME              CPU(cores)   CPU%      MEMORY(bytes)   MEMORY%  
u-k8s-worker-04   173m         4%        4108Mi          56%      
NAME              CPU(cores)   CPU%      MEMORY(bytes)   MEMORY%  
u-k8s-worker-05   290m         7%        5014Mi          69%      
NAME              CPU(cores)   CPU%      MEMORY(bytes)   MEMORY%  
u-k8s-worker-06   164m         4%        3388Mi          46%      
NAME              CPU(cores)   CPU%      MEMORY(bytes)   MEMORY%  
u-k8s-worker-07   133m         3%        2619Mi          36%      
NAME              CPU(cores)   CPU%      MEMORY(bytes)   MEMORY%  
u-k8s-worker-08   186m         4%        5500Mi          76%    

# Copy directory from pod
```
#copy .ssh directory into a local .ssh directory
kubectl cp sdlc/ubuntu-agent-2786121838-xxpkf:/root/.ssh .ssh
```

# Autoscaling

First we need to create a Limit Range. This is due to the autoscaling algorithm using the cpu requests to decide when to scale. For example if we decide to scale when we hit 50% of the cpu, that 50% will be decided towards the requested cpu. 

A Limit Range provides *default* values within a namespace. Any particular container within a pod definition can overwrite the limit range values.

limitrange.yaml content
```
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-mem-limit-range
spec:
  limits:
  - default:
      cpu: 0.5
      memory: "0.5Gi"
    defaultRequest:
      cpu: 0.05
      memory: "256M"
    type: Container
```

First make sure to be in your desired namespace with switching your context or using the -n NAMESPACE flag. 

Then, apply the limit range. 

```
k apply -f limitrange.yaml
```

Now you are ready to setup autoscaling to any particular deployment. Pick your deployment, php-apache in our case, cpu target values and minimum to maximum number of deployments according to our load. The autoscaling algorithm will increase pods faster than it decreases. This is to avoid noise and given that there is generaly less rush increasing capacity rather than decreasing it.

Quoting the documentation (#2): "Starting and stopping pods may introduce noise to the metric (for instance, starting may temporarily increase CPU). So, after each action, the autoscaler should wait some time for reliable data. Scale-up can only happen if there was no rescaling within the last 3 minutes. Scale-down will wait for 5 minutes from the last rescaling." 

We apply our autoscaling policy: 
```
```
If your pod existed before-hand and didn't have any cpu request, make sure to recreate so it gets the default namespace cpu target.





References: 
* #1 http://blog.kubernetes.io/2016/07/autoscaling-in-kubernetes.html
* #2 https://github.com/kubernetes/kubernetes/blob/8caeec429ee1d2a9df7b7a41b21c626346b456fb/docs/design/horizontal-pod-autoscaler.md#autoscaling-algorithm
* #3 https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/
