# Day 35/40 - Kubernetes ETCD Backup And Restore Explained

## Check out the video below for Day35 👇

[![Day 35/40 - Kubernetes ETCD Backup And Restore Explained](https://img.youtube.com/vi/R2wuFCYgnm4/sddefault.jpg)](https://youtu.be/R2wuFCYgnm4)

### Install Etcd Client
``` bash
sudo apt install etcd-client
```
### Set Env 
``` bash
export ETCDCTL_API=3
```
### Search Flags
``` bash
sudo cat /etc/kubernetes/manifests/etcd.yaml
```
`--listen-client-urls=https://127.0.0.1:2379` <---

`--trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt` <---

`--cert-file=/etc/kubernetes/pki/etcd/server.crt` <---

`--key-file=/etc/kubernetes/pki/etcd/server.key` <---
### Create Snapshot with Flags
``` bash
sudo ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
snapshot save /opt/etcd-backup.db
```
### Check Snapshot Size
``` bash
du -sh /opt/etcd-backup.db
```
[References](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster)

### Restore
``` bash
sudo ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
--cacert=/etc/kubernetes/pki/etcd/ca.crt \
--cert=/etc/kubernetes/pki/etcd/server.crt \
--key=/etc/kubernetes/pki/etcd/server.key \
snapshot restore /opt/etcd-backup.db \
--data-dir=/var/lib/etcd-restore-from-backup
```
### Change Dir on etcd.yaml
``` bash
sudo nano /etc/kubernetes/manifests/etcd.yaml
```
1. `--data-dir=/var/lib/etcd` to `--data-dir=/var/lib/etcd-restore-from-backup`
2. `- mountPath: /var/lib/etcd` to `- mountPath: /var/lib/etcd-restore-from-backup`
3. `path: /var/lib/etcd` to `path: /var/lib/etcd-restore-from-backup` 
### Restart All Component on Master Node
``` bash
sudo mv /etc/kubernetes/manifests/*.yaml /tmp
```
``` bash
sudo mv /tmp/*.yaml /etc/kubernetes/manifests/
```
``` bash
ls /etc/kubernetes/manifests/
```
### Restart Kubelet
``` bash
sudo systemctl daemon-reload
```
``` bash
sudo systemctl restart kubelet
```
