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
