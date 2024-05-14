
# Installing additional components

## Install [Helm](https://helm.sh/)
Helm is the package manager for Kubernetes using which, you can install any component to your cluster. 
You can install it on your local machine or on your Kubernetes cluster using the [docs](https://helm.sh/docs/intro/install/)

## Nginx Ingress Controller
Kubernetes does not implement load balancer service by default and if you want to deploy public services to your cluster, 
you will need to install the Ingress Controller which will route requests coming to your cluster based on request url. 
In most cases people will suggest you to install MetalLB as a load balancer if you are self-hosting kubernetes cluster,
but traditional hosting providers give one IP for each node, and it does not make sense to acquire additional IPs to 
just satisfy the load balancer (of course if you are not planning to put all your nodes behind private networks and use 
different IPs for each ingress controller service). Instead of installing MetalLB you can install Ingress Controller 
itself on host network or exposing it using NodePort service.

### Deploy Ingress Controller on host network IF
You want it to listen on traditional HTTP and HTTPS ports 80 and 443 respectively. Kubernetes does not allow you to use ports outside the range of 30000-32767. 
Enabling host network, you will allow your pods to access local server's local network and listen standard ports. 
This is good if you are planning to deploy public services, APIs or frontend applications that must be available at traditional ports
But in this case you cannot deploy replica of this ingress controller on the same node, because only one process can 
listen on the same port. 

### Deploy Ingress Controller on NodePort service
If you don't require standard ports and local network it's better to publish your services using NodePort service. 
In this case kube-proxy will expose your service on one of the allowed ports and you can access it using NodePort service.

Learn more about this [here](https://kubernetes.github.io/ingress-nginx/deploy/baremetal/)

Most common Ingress controller for kubernetes cluster is NGINX Ingress Controller, so we will use it.
