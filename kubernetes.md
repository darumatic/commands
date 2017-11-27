#Create Docker Registry Secret
```
kubectl create secret docker-registry nexus --docker-server=localhost:5000 --docker-username=admin --docker-password= --docker-email=hello@darumatic.com
```

#Kubernetes components quick health check
```
kubectl get componentstatuses
```

#Memory / resource check
```
for i in `k get nodes |grep -v NAME|cut -d ' ' -f 1`; do k top node $i;done
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
