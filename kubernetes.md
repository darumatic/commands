# Create Docker Registry Secret
```
kubectl create secret docker-registry nexus --docker-server=localhost:5000 --docker-username=admin --docker-password= --docker-email=hello@darumatic.com
```

# Create TLS Certificate
```
kubectl create secret tls your_tls_name --cert=your_cert_file.crt --key=your_key_file.key -n your_namespace
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

# Visualising pods with their containers and versions in a particular namespace

```
$kubectl -n kube-system get --no-headers=true pods -o custom-columns=:metadata.name,:status.phase,:.spec.containers[*].image 
calico-kube-controllers-3930078641-twk7l   Running   quay.io/calico/kube-controllers:v1.0.0
calico-node-4bxsx                          Running   quay.io/calico/node:v2.6.2,quay.io/calico/cni:v1.11.0
calico-node-6wp5w                          Running   quay.io/calico/node:v2.6.2,quay.io/calico/cni:v1.11.0
calico-node-9nvxn                          Running   quay.io/calico/node:v2.6.2,quay.io/calico/cni:v1.11.0
calico-node-gxr31                          Running   quay.io/calico/node:v2.6.2,quay.io/calico/cni:v1.11.0
calico-node-jkrl5                          Running   quay.io/calico/node:v2.6.2,quay.io/calico/cni:v1.11.0
calico-node-ps231                          Running   quay.io/calico/node:v2.6.2,quay.io/calico/cni:v1.11.0
```

# k alias

This is what you need to add to your ~/.bashrc to get the command auto completion working with "k" alias:
```
alias k="kubectl"
source <(kubectl completion bash)
complete -o default -F __start_kubectl k
```
