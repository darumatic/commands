# Execute ad-hoc command as the root user in a particular host

```
ansible -i onegov_uat_cluster.cfg -b --become-user=root -u k8user u-k8s-worker-01 -a "service docker restart"
```

# Running playbook to adjust docker registry settings
```
ansible-playbook -i onegov_uat_cluster.cfg -b --become-user=root -u k8user insedocker.yaml
```

Insedocker.yaml
```
---
- hosts: u-k8s-worker-01
  tasks:
  - name: copy daemon
    copy:
        src: daemon.json
        dest: /etc/docker/daemon.json
  - name: Creates directory
    file: path=/root/.docker state=directory
  - name: copy docker config
    copy:
        src: config.json
        dest: /root/.docker/config.json
  - name: restart docker
    service: name=docker state=restarted


```
