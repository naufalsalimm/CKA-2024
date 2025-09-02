### Membuat manifest Kubernetes langsung dari shell
```bash
cat << EOF | kubectl apply -f -
<isi yaml manifest>
EOF
```
Example:
```bash
cat << EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.25
EOF
```

### Menghilangkan taint di node control-plane
``` bash
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```
### Tes Nslookup 
``` bash
kubectl run dnsutils --rm -it --image=busybox:1.28 --restart=Never -- nslookup google.com
```
### Melihat daftar context
```
kubectl config get-contexts
```
### Mengganti context
```
kubectl config use-context <nama-context>
```
