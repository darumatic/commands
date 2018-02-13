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

# Checking failing pods in different clusters
This command analyse failing pods (not in 'Running state') across different clusters. 

Please replace the configuration files: c1-uat,c1-prod,c2-prod,c2-uat accordingly
```bash
for i in {c1-uat,c1-prod,c2-prod,c2-uat} ;do echo $i;k --kubeconfig ~/.kube/config-$i get pods --all-namespaces -o wide|grep -v Running;done

#Output:
#c1-uat
#NAMESPACE            NAME                                       READY     STATUS    RESTARTS   AGE       IP               NODE
#c1-prod
#NAMESPACE            NAME                                       READY     STATUS    RESTARTS   AGE       IP               NODE
#c2-prod
#NAMESPACE            NAME                                       READY     STATUS    RESTARTS   AGE       IP               NODE
#c2-uat
#NAMESPACE            NAME                                       READY     STATUS    RESTARTS   AGE       IP              NODE
adrian@adrian-ubuntu-XPS:~$ 

```

