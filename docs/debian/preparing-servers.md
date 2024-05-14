# Preparing Servers for Kubernetes Cluster (Debian)

To be able to run kubernetes we need to install some packages. This document will guide you through the process of 
preparing your servers for Kubernetes installation.

## Updating the system
First of all let’s configure the fresh debian server installing common packages

```bash
# Disable swap
sudo swapoff -a

sudo apt-get update
sudo apt-get upgrade
```
Install essential packages

```bash
apt install sudo curl wget git vim nano ipvsadm htop ca-certificates apt-transport-https software-properties-common gnupg lsb-release -y
```

Enable port forwarding

```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
EOF

# Apply sysctl params without reboot
sudo sysctl --system
```

Verify that `net.ipv4.ip_forward` is set to 1 with:

```bash
sysctl net.ipv4.ip_forward
```

## Installing Container Runtime Interface
There are two options for the container runtime interface (CRI) that you can choose from:
- Docker
- Containerd
- CRI-O
- Mirantis Container Runtime

For official documentation refer to [CRI Docs here](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
In this documentation we will use containerd for CRI.

The `containerd.io` packages in DEB and RPM formats are distributed by Docker (not by the containerd project). 
See the Docker documentation for more information [here](https://docs.docker.com/engine/install/)

Before you install `containerd` for the first time on a new host machine, you need to set up the 
Docker `apt` repository. Afterward, you can install and update `containerd` from the repository

```bash
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
 
sudo apt-get update

# we don't need other docker packages since we are using only containerd for kubernetes.
sudo apt-get install containerd.io
```

Next step is to check and configure cgroup driver. Since we are installing kubernetes with kubeadm, 
we will use `systemd` as our cgroup driver. `systemd` is the default driver, for debian based systems but 
containerd uses `cgroupfs` by default. To avoid issues with containerd, we will configure it to use systemd

Check and verify that `systemd` is the cgroup driver with:
```bash
ps -p 1

# Output
    PID TTY          TIME CMD
      1 ?        00:00:01 systemd
```

Now let's dump containerd default configs and update it to use `systemd`

```bash
sudo containerd config default > /etc/containerd/config.toml
```

Open the file `/etc/containerd/config.toml` and update the following lines:
```bash
# Find the SystemdCgroup line under [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options] and update it to `SystemdCgroup = true`
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true
```

Optionally you can update sandbox_image version to make kubeadm happy during cluster initialization.
The suggested version for kubernetes v1.30 was pause:3.9 pay attention to kubeadm outputs when initializing the cluster. (can be done later)

Restart `containerd` service

```bash
sudo systemctl restart containerd
```

## Done
Keep in mind the above steps should be done in all servers that will be used for kubernetes cluster.
Next step is to [install kubeadm and kubelet on your servers](../installing-kubeadm.md)
