## Installing kubeadm, kubelet and kubectl
###### Update the apt package index and install packages needed to use the Kubernetes apt repository:
```
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl
```

###### Download the Google Cloud public signing key:
```shell
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
```

###### Add the Kubernetes `apt` repository:
```shell
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

###### Update apt package index, install kubelet, kubeadm and kubectl, and pin their version:
```
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

###### Creating a cluster with kubeadm
__Master node__
```shell
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config 
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
__Worker node__
```shell
sudo kubeadm join <ip_v4_private>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash-string>
```

###### Deploy Pod Network – Flannel
```shell
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
__Result__
