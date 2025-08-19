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
