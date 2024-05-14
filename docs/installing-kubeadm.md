
# Installing Kubeadm

Download the public signing key for the Kubernetes package repositories. The same signing key is used for all 
repositories so you can disregard the version in the URL:

```bash
# If the directory `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
# sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

Don't forget to update the version of the package in the above command. Current version is `v1.30`
Add the appropriate Kubernetes `apt` repository. Please note that this repository have packages only for Kubernetes 
1.30; for other Kubernetes minor versions, you need to change the Kubernetes minor version in the URL to match your 
desired minor version (you should also check that you are reading the documentation for the version of Kubernetes that 
you plan to install). Official docs can be found [here](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

```bash
# This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
Update the `apt` package index, install kubelet, kubeadm and kubectl, and pin their version

```bash
sudo apt-get update

# For the exact version use the following command: 
# sudo apt-get install -y kubelet=1.30.0-00 kubeadm=1.30.0-00 kubectl=1.30.0-00
sudo apt-get install -y kubelet kubeadm kubectl
```

Next step is to [initialize the cluster](./initializing-cluster.md)
