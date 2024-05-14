## Install ingress-nginx using Helm

```bash
# Add repository
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

# Install chart
helm install <name>-ingress-nginx ingress-nginx/ingress-nginx
```


To install ingress-nginx with custom values run the command
```bash
helm install <name>-ingress-nginx ingress-nginx/ingress-nginx --values values.yaml
```
