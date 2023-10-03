# k3s-rbac

Generate certificate for user

```
openssl genrsa -out phani.key 2048
openssl req -new -key  phani.key -out phani.csr -subj "/CN=phani/O=phani"
openssl x509 -req -in  phani.csr  -CA /var/lib/rancher/k3s/server/tls/client-ca.crt -CAkey /var/lib/rancher/k3s/server/tls/client-ca.key  -CAcreateserial -out phani.crt  -days 500
```
Create Kube config file and define the certs path

NOTE: certificate-authority-data you can get from existing kubeconfig

```
apiVersion: v1
clusters:
- cluster:
   certificate-authority-data: DQVIyZ0F3SUJBZ0lCQURBS0JnZ3Foa2pPUFFRREFqQWpNU0V3SHdZRFZRUUREQmhyTTNNdGMyVnkKZG1WeUxXTmhRREUyT1RNME5UVTRNVGd3SGhjTk1qTXdPRE14TURReU16TTRXaGNOTXpNd09ESTRNRFF5TXpNNApXakFqTVNFd0h3WURWUVFEREJock0zTXRjMlZ5ZG1WeUxXTmhRREUyT1RNME5UVTRNVGd3V1RBVEJnY3Foa2pPClBRSUJCZ2dxaGtqT1BRTUJCd05DQUFRZGZXdTNzakZHc2tKRjk1MmxNcXVQM1ZsOTdMdVV0UHljdHBsbGZMejMKVGtVNG9MTjhQTXBrby9qUmZXUS9oUGdJZlR6ZExwcWdBRzlBQW0vYlpJOW9vMEl3UURBT0JnTlZIUThCQWY4RQpCQU1DQXFRd0R3WURWUjBUQVFIL0JBVXdBd0VCL3pBZEJnTlZIUTRFRmdRVWtkbmlJaFRHTktvamsrVGp0N1ppCmUrL1VLT0V3Q2dZSUtvWkl6ajBFQXdJRFNBQXdSUUlnR0F1WGI5a1h6OERrbEZheHd1dm43QTh1aFBBRnUxczAKa1grL01HSEszUllDSVFEclJ6bHppTkY4dTcwVTVDbFl3Qy9IN2xDR2RGT29RN29wbUpTRERhY0UvUT09C
   server: https://127.0.0.1:6443
  name: default
contexts:
- context:
   cluster: default
   user: phani
  name: phani-context
current-context: phani-context
kind: Config
preferences: {}
users:
- name: phani
  user:
   client-certificate: /home/phani/cert/phani.crt
   client-key: /home/phani/cert/phani.key
```

Add required alias to ~/.bashrc

```
export KUBECONFIG=/home/phani/.kube/config
n=default
alias k='kubectl -n $n'
alias kd='kubectl -n $n describe'
alias kg='kubectl -n $n get'
alias kl='kubectl -n $n logs'
alias klf='kubectl -n $n logs -f --since=5m'
alias kpd='kubectl -n $n delete pod'
alias kpdf='kubectl -n $n delete pod --force --grace-period=0'
```

```
source ~/.bashrc
```
