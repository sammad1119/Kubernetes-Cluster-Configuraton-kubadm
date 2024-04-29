# Kubernetes v1.28 cluster Setup on unduntu Using Kubeadm and Containerd

## Prerequisites

- A compatible Linux hosts:  2 GB or more of RAM per machine and 2 CPUs or more 
- 3 - Ubuntu 20.04 LTS Serves:  1x Manager (4GB RAM, 2 vCPU)t2.medium type, 2x Workers (1 GB, 1 Core) t2.micro type 
- Full network connectivity between all machines in the cluster 
- Unique hostname for each host. Change hostname of the machines using hostnamectl. For master nodes, run`hostnamectl set-hostname master`. For slaves, run `hostnamectl set-hostname slave-01`  `hostnamectl set-hostname slave-02` 
- Certain ports are open on your machines(https://kubernetes.io/docs/reference/ports-and-protocols/)
  - On Master Node
	```
	6443/tcp for Kubernetes API Server
	2379-2380 for etcd server client API
	6783/tcp,6784/udp for Weavenet CNI
	10248-10260 for Kubelet API, Kube-scheduler, Kube-controller-manager, Read-Only Kubelet API, Kubelet health
	80,8080,443 Generic Ports
	30000-32767 for NodePort Services
	```
  - On Slave Nodes
	```
	6783/tcp,6784/udp for Weavenet CNI
	10248-10260 for Kubelet API etc
	30000-32767 for NodePort Services
	```

## Run on all nodes of the cluster as root user
#### Disable SWAP
You MUST disable swap in order for the kubelet to work properly 
```
swapoff -a
sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```
#### Install Containerd or Docker
```
wget https://github.com/containerd/containerd/releases/download/v1.7.13/containerd-1.7.13-linux-amd64.tar.gz
tar Cxzvf /usr/local containerd-1.7.13-linux-amd64.tar.gz
wget https://raw.githubusercontent.com/containerd/containerd/main/containerd.service
mkdir -p /usr/local/lib/systemd/system
mv containerd.service /usr/local/lib/systemd/system/containerd.service
systemctl daemon-reload
systemctl enable --now containerd

or

sudo apt intall docker 
```



#### Install kubectl, kubelet and kubeadm
```
apt-get update
apt-get install -y apt-transport-https ca-certificates curl gpg

mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

apt-get update -y
apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```
# Check kubeadm version
```
kubeadm version
```
```
kubeadm version: &version.Info{Major:"1", Minor:"29", GitVersion:"v1.28.9", GitCommit:"4b8e819355d791d96b7e9d9efe4cbafae2311c88", GitTreeState:"clean", BuildDate:"2024-02-14T10:39:04Z", GoVersion:"go1.21.7", Compiler:"gc", Platform:"linux/amd64"}
```
# pod Communication  or Install addon 

Calico : Choose one node as your Kubernetes master. On that node sudo kubeadm init --pod-network-cidr=192.168.0.0/16



## Run on Master Node and follow the instructions

```
kubeadm config images pull
kubeadm init
```
#### Install any CNI plugin. We will use weavenet
```
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

## Run on Slave Nodes 
Run the join command obtained from kubeadm init output on all Workers nodes. Example
```
kubeadm join \
192.168.56.2:6443 --token … --discovery-token-ca-cert-hash sha256 . . . .
```

## Test the setup
```
kubectl get nodes
NAME       STATUS   ROLES           AGE     VERSION
master     Ready    control-plane   3m59s   v1.28.9
slave-01   Ready    <none>          3m19s   v1.28.9

******
```
```
kubectl get pods -A
```

## Run a demo app
```
kubectl run nginx --image=nginx --port=80 
kubectl expose pod nginx --port=80 --type=NodePort
```

## References
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
- https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/
- https://www.mirantis.com/blog/how-install-kubernetes-kubeadm/
- https://www.mankier.com/1/kubeadm-init
- https://kubernetes.io/docs/setup/production-environment/container-runtimes/#docker
- https://github.com/containerd/containerd/blob/main/docs/getting-started.md
- https://kubernetes.io/docs/reference/networking/ports-and-protocols/
- https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#install
- https://github.com/skooner-k8s/skooner
- https://www.weave.works/docs/net/latest/kubernetes/kube-addon/#eks
- https://github.com/kubernetes-sigs/cri-tools/blob/master/docs/crictl.md
- https://www.mankier.com/1/kubeadm-init
