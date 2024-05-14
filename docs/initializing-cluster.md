
# Initializing the Kubernetes Cluster

After successfully [installing the kubeadm](installing-kubeadm.md), now we can start the cluster.

Run the following command ONLY on the master node
Based on the CNI plugin you selected, you will need to initialize the cluster with an appropriate Pod Network CIDR.
For Calico it is `192.168.0.0/16`
For Flannel it is `10.244.0.0/16` and
For Weave default cidr block is `10.32.0.0/12`
Check the [docs](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#initializing-your-control-plane-node) for more details
```bash
sudo kubeadm init #--pod-network-cidr=192.168.0.0/16
```

Kubeadm will start the cluster and necessary services and outputs some important information on how to join other 
nodes to cluster.

BUT before you do that, quickly install your [CNI plugin](./installing-additional-components.md) and come back here.

## Install CNI plugin

One of the common CNI plugins that you can use is [Calico](https://docs.tigera.io/calico/latest/getting-started/)
You can use other plugins like [Flannel](https://github.com/flannel-io/flannel#deploying-flannel-manually), 
[Weave](https://github.com/weaveworks/weave/blob/master/site/kubernetes/kube-addon.md#-installation), [Cilium](https://cilium.io/), etc.
For details on kubernetes addons refer to [docs here](https://kubernetes.io/docs/concepts/cluster-administration/addons/)

## Joining Nodes to Cluster

To join other nodes to the cluster, run the command suggested by kubeadm in control plane node, if somehow you cleared 
or missed the command, you can follow these steps to generate token.

First list the generated join tokens
```bash
kubeadm token list
```

If you don't have the value of --discovery-token-ca-cert-hash, you can get it by running the following command chain on the control-plane node:

```bash
openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | \
   openssl dgst -sha256 -hex | sed 's/^.* //'
```

Run the following command on all nodes that you want to join the cluster:
```bash
kubeadm join --token <token> <control-plane-host>:<control-plane-port> --discovery-token-ca-cert-hash sha256:<hash>
```

By default, tokens expire after 24 hours. If you are joining a node to the cluster after the current token has expired, 
you can create a new token by running the following command on the control-plane node:

```bash
kubeadm token create --print-join-command
```

Now it's time to add [Nginx Ingress Controller and Cert Manager to the cluster](installing-additional-components.md)
