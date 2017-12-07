# Execute ad-hoc command as the root user in a particular host

```
ansible -i onegov_uat_cluster.cfg -b --become-user=root -u k8user u-k8s-worker-01 -a "service docker restart"
```

