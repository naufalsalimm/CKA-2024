# Day 21 - Create Certificate Signing Request
openssl genrsa -out andhika.key 2048

openssl req -new -key andhika.key -out andhika.csr -subj "/CN=andhika"

cat andhika.csr | base64 | tr -d '\n'

nano csr.yaml

```
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: andhika-csr
spec:
  request: <csr-base64>
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
```

k apply -f csr.yaml

k certificate approve andhika-csr

# Role Based Access Control Kubernetes

nano role.yaml

k apply -f role.yaml

nano role-binding.yaml

k apply -f role-binding.yaml

kubectl get csr andhika-csr -o jsonpath='{.status.certificate}'| base64 -d > andhika.crt

k config set-credentials andhika --client-key=andhika.key --client-certificate=andhika.crt

k config set-context andhika --cluster=default --user=andhika

k config use-context andhika

### Observability commands
kubectl auth can-i get pod
kubectl auth whoami
kubectl auth can-i get pod --as andhika
