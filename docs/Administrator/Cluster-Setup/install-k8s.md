# Native Kubeneretes Installation

Kubernetes is composed of master(s) and workers. The instructions below are for creating a bare-bones installation of a single master and a number of workers. For a more complex Kubernetes installation, please use tools such as _Kubespray_ [https://kubespray.io/](https://kubespray.io/#/).

## Prerequisites:

All machines have Ubuntu 18.04



## Run on Master Node

Install docker by performing the instructions [here](https://docs.docker.com/engine/install/ubuntu/). Specifically you can use a convenience script provided in the document:
``` shell
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

Restart the docker service:

`sudo systemctl restart docker`


Install Kubernetes master:
``` shell
sudo sh -c 'cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF'

sudo apt-get update && sudo apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
sudo apt-get update
sudo apt-get install -y kubelet=1.18.4-01 kubeadm=1.18.4-01 kubectl=1.18.4-01

swapoff -a
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --kubernetes-version=v1.18.4
```

Save the output of the `kubeadm init` command.

``` shell
mkdir .kube
sudo cp -i /etc/kubernetes/admin.conf .kube/config
sudo chown $(id -u):$(id -g) .kube/config

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

```

Test that Kubernetes is up and running:

```
kubectl get nodes
```
Verify that the master node is ready



## Run on Kubernetes Workers

Install docker by performing the instructions here: https://docs.docker.com/engine/install/ubuntu/. Specifically you can use a convenience script provided in the document:
``` shell
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

Restart the docker service:

`sudo systemctl restart docker`


Install Kubernetes worker:
``` shell
sudo sh -c 'cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF'

sudo apt-get update && sudo apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
sudo apt-get update
sudo apt-get install -y kubelet=1.18.4-01 kubeadm=1.18.4-01

swapoff -a
```

Replace the following `join` command with the one saved from the init command above:

``` shell
sudo kubeadm join 10.0.0.3:6443 --token <token> \
    --discovery-token-ca-cert-hash sha256:<hash>
```

## Permanently disable swap on all nodes

1. Edit the file /etc/fstab
2. Comment out any swap entry if such exists

