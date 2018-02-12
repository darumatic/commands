In this document I will explain how to setup auto-scaling as per Kubernetes 1.7. 

Only pod scaling is covered but no node scaling which depends on the integration of Kubernetes and your cloud provider to provision new nodes.

# Setting up the pod cpu request

First we need to setup the cpu requests. This is due to the autoscaling algorithm using the cpu requests to decide when to scale. For example if we decide to scale when we hit 50% of the cpu, that 50% will be decided towards the requested cpu and not actually a full cpu unit as per the 'top' command in bash. 

This is how we specify a cpu request limit. Additionally I'm setting up the cpu limits and memory request and limits. Cpu and memory requests provides Kubernetes enough information to know how many pods can fit on each node. Limits are passed to the Docker engine which in turns uses Cgroups to control the resource limits of each container.   

This is the detail of the deployment yaml file that defines a pod and its limits.

```
      - name: filebeat
        image: komljen/filebeat
        resources:
          requests:
            memory: "256M"
            cpu: "0.02"
          limits:
            memory: "0.5Gi"
            cpu: "0.2"
```

An alternative to individual limit is to create a Limit Range object. 

A Limit Range provides *default* values within a namespace. Any particular container within a pod definition can overwrite the limit range values. One clarification though, in my test as per Kubernetes 1.7 the limit range values would not be picked up by the auto-scaler even after recreating the deployment. In any case, this is how it looks like:  

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

# Autoscaling

Once you setup the cpu request, you are ready to setup autoscaling. But before, you need to decide how much cpu your pods should consume in order to trigger a scale order (cpu target value) Also you need to setup the maximum and minimum limits for your pods to scale up or down. Unfortunately you can't scale to infinite but surely to a very large number of pods. 

The autoscaling algorithm will increase pods faster than it decreases. This is to avoid noise and given that there is generally less rush increasing capacity rather than decreasing it.

Quoting the documentation (#2): "Starting and stopping pods may introduce noise to the metric (for instance, starting may temporarily increase CPU). So, after each action, the autoscaler should wait some time for reliable data. Scale-up can only happen if there was no rescaling within the last 3 minutes. Scale-down will wait for 5 minutes from the last rescaling." 

We apply our autoscaling policy: 

```bash
kubectl autoscale deployment file-beat --cpu-percent=50 --min=1 --max=5
```
In this case we are specifying to scale the file-beat deployment once the cpu load hits 50% of the cpu requested and up to 5 pods.

In order for this to work, make sure to specify a cpu target in all the containers within your deployment. To verify this, just run:

```bash
adrian@adrian-ubuntu-XPS:~$ kubectl get hpa
NAME         REFERENCE                            TARGETS             MINPODS   MAXPODS   REPLICAS   AGE
myapp        Deployment/myapp                     <unknown> / 50%     1         5         1          6h
app2         Deployment/app2                      0% / 50%            1         5         1          5h
```

In the example above myapp doesn't have cpu target defined properly while app2 does. Therefore we can get autoscaling to properlt work for app2. 


# Test

To simulate load within a pod you can run:

```
fulload() { dd if=/dev/zero of=/dev/null | dd if=/dev/zero of=/dev/null | dd if=/dev/zero of=/dev/null |dd if=/dev/zero of=/dev/null | dd if=/dev/zero of=/dev/null | dd if=/dev/zero of=/dev/null | dd if=/dev/zero of=/dev/null & }; fulload; read; killall dd
```

Other commands to help you out:

```bash
#to verify number of pods
kubectl get pods -n dev |grep app

#to get status updates from the auto-scaler
k get hpa app2 -o yaml
```






#References

* #1 http://blog.kubernetes.io/2016/07/autoscaling-in-kubernetes.html
* #2 https://github.com/kubernetes/kubernetes/blob/8caeec429ee1d2a9df7b7a41b21c626346b456fb/docs/design/horizontal-pod-autoscaler.md#autoscaling-algorithm
* #3 https://kubernetes.io/docs/tasks/administer-cluster/memory-default-namespace/
