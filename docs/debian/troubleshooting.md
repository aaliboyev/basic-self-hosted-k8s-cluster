
## Kubeadm throws error: "bridge-nf-call-iptables does not exist"
You can fix it running the following commands
```bash
modprobe br_netfilter
echo 1 > /proc/sys/net/bridge/bridge-nf-call-iptables
echo 1 > /proc/sys/net/ipv4/ip_forward
```
