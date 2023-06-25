### Getting started

### Kubernetes,

**Getting started ,** https://kubernetes.io/docs/setup/

**Install Tools ,** https://kubernetes.io/docs/tasks/tools/

**Installing Kubernetes with deployment tools ,** https://kubernetes.io/docs/setup/production-environment/tools/


### KUBEADM,

**Bootstrapping clusters with kubeadm ,** https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/

**Installing kubeadm ,** https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

<br>

### System configuration,

**Keep firewalld and SELinux turned off,**

`anup@ubuntu-22042-08:~$ sudo systemctl status ufw.service `

`anup@ubuntu-22042-08:~$ ufw status `


**For : [WARNING Swap]: swap is enabled; production deployments should disable swap unless testing the NodeSwap feature gate of the kubelet**

`anup@ubuntu-22042-08:~$ sudo swapoff -a `


<br>

<br>

### Go

**Go Download,** https://go.dev/doc/install

`anup@ubuntu-22042-08:~$ wget https://go.dev/dl/go1.20.5.linux-amd64.tar.gz`


### Go installation,

**Remove any previous Go installation,**

`anup@ubuntu-22042-08:~$ sudo mkdir /home/anup/go`

`anup@ubuntu-22042-08:~$ rm -rf /home/anup/go && tar -C /home/anup -xzf go1.20.5.linux-amd64.tar.gz`

<br>

**Add /usr/local/go/bin to the PATH environment variable,**

`anup@ubuntu-22042-08:~$ export PATH=$PATH:/home/anup/go/bin`

`anup@ubuntu-22042-08:~$ echo $PATH`

<br>

**Verify installation,**

`anup@ubuntu-22042-08:~$ go version`


<br>

<br>

### Docker Engine , https://docs.docker.com/engine/install/ubuntu/


### Run the following command to uninstall all conflicting packages,

    anup@ubuntu-22042-08:~$ docker version
    
    anup@ubuntu-22042-08:~$ for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

### Install using the apt repository,

**Set up the repository,**

    anup@ubuntu-22042-08:~$ sudo apt-get update
    
    anup@ubuntu-22042-08:~$ sudo apt-get install ca-certificates curl GnuPG

<br>

    anup@ubuntu-22042-08:~$ sudo install -m 0755 -d /etc/apt/keyrings
    
    anup@ubuntu-22042-08:~$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    
    anup@ubuntu-22042-08:~$ sudo chmod a+r /etc/apt/keyrings/docker.gpg

<br>

    anup@ubuntu-22042-08:~$ echo \
      "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

**Install Docker Engine,**

    anup@ubuntu-22042-08:~$ sudo apt-get update
    
    anup@ubuntu-22042-08:~$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

<br>

    anup@ubuntu-22042-08:~$ docker --version
    
    anup@ubuntu-22042-08:~$ sudo systemctl status docker.service 
    
    anup@ubuntu-22042-08:~$ sudo systemctl enable docker.service 
    
    anup@ubuntu-22042-08:~$ sudo docker run hello-world


### Manage Docker as a non-root user,

    anup@ubuntu-22042-08:~$ sudo groupadd docker
    
    anup@ubuntu-22042-08:~$ sudo usermod -aG docker $USER
    
    anup@ubuntu-22042-08:~$ newgrp docker
    
    anup@ubuntu-22042-08:~$ docker run hello-world


<br>

<br>

### Installing Container Runtime Interface (CRI), https://kubernetes.io/docs/setup/production-environment/container-runtimes/

**Docker Engine, **https://kubernetes.io/docs/setup/production-environment/container-runtimes/#docker

1. On each of your nodes, install Docker for your Linux distribution as per Install Docker Engine, https://docs.docker.com/engine/install/#server

2. Install cri-dockerd, following the instructions in that source code repository, https://github.com/Mirantis/cri-dockerd,

**Note :** For cri-dockerd, the CRI socket is unix:///var/run/cri-dockerd.sock by default.

<br>

`anup@ubuntu-22042-08:~$ sudo apt-get update`

`anup@ubuntu-22042-08:~$ sudo apt-get install git`

`anup@ubuntu-22042-08:~$ git clone https://github.com/Mirantis/cri-dockerd.git`

`anup@ubuntu-22042-08:~$ cd cri-dockerd/`

`anup@ubuntu-22042-08:~/cri-dockerd$ sudo apt-get update`

`anup@ubuntu-22042-08:~/cri-dockerd$ sudo apt-get install make`

`anup@ubuntu-22042-08:~/cri-dockerd$ make cri-dockerd`

<br>

`[anup@rhel-92-04 cri-dockerd]$ mkdir -p /usr/local/bin`

`[anup@rhel-92-04 cri-dockerd]$ sudo install -o root -g root -m 0755 cri-dockerd /usr/local/bin/cri-dockerd`

`[anup@rhel-92-04 cri-dockerd]$ sudo install packaging/systemd/* /etc/systemd/system`

`[anup@rhel-92-04 cri-dockerd]$ sudo sed -i -e 's,/usr/bin/cri-dockerd,/usr/local/bin/cri-dockerd,' /etc/systemd/system/cri-docker.service`

<br>

`[anup@rhel-92-04 cri-dockerd]$ sudo systemctl daemon-reload`

`[anup@rhel-92-04 cri-dockerd]$ sudo systemctl status cri-docker.service`

`[anup@rhel-92-04 cri-dockerd]$ sudo systemctl start cri-docker.service`

`[anup@rhel-92-04 cri-dockerd]$ sudo systemctl enable cri-docker.service`

`[anup@rhel-92-04 cri-dockerd]$ sudo systemctl status --now cri-docker.socket`

`[anup@rhel-92-04 cri-dockerd]$ sudo systemctl enable --now cri-docker.socket`

`[anup@rhel-92-04 cri-dockerd]$ ls -ltr /var/run/cri-dockerd.sock `

`[anup@rhel-92-04 cri-dockerd]$ cd`

<br>

<br>

### Installing kubeadm, kubelet and kubectl , Red Hat 9 , https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl

<br>

**Update the apt package index and install packages needed to use the Kubernetes apt repository ,**

`anup@ubuntu-22042-08:~$ sudo apt-get update`

`anup@ubuntu-22042-08:~$ sudo apt-get install -y apt-transport-https ca-certificates curl`

<br>

**Download the Google Cloud public signing key ,**

`anup@ubuntu-22042-08:~$ curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg`

<br>

**Add the Kubernetes apt repository ,**

`anup@ubuntu-22042-08:~$ echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list`

<br>

**Update apt package index, install kubelet, kubeadm and kubectl, and pin their version ,**

`anup@ubuntu-22042-08:~$ sudo apt-get update`

`anup@ubuntu-22042-08:~$ sudo apt-get install -y kubelet kubeadm kubectl`

`anup@ubuntu-22042-08:~$ sudo apt-mark hold kubelet kubeadm kubectl`

<br>

`anup@ubuntu-22042-08:~$ kubelet --version`

`anup@ubuntu-22042-08:~$ kubeadm version`

`anup@ubuntu-22042-08:~$ kubectl version`

<br>

<br>


### Configuring a cgroup driver,

**Not required for Docker CRI**

<br>

<br>

### Creating a Cluster with kubeadm,

`anup@ubuntu-22042-08:~$ kubeadm init`

`anup@ubuntu-22042-08:~$ sudo kubeadm init --pod-network-cidr=10.32.0.0/12 --apiserver-advertise-address=192.168.56.4 --cri-socket=unix:///var/run/cri-dockerd.sock (Weave Net)`

or,

`anup@ubuntu-22042-08:~$ sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=192.168.56.4 --cri-socket=unix:///var/run/cri-dockerd.sock (flannel)`

or,

`anup@ubuntu-22042-08:~$ sudo kubeadm init --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=192.168.56.4 --cri-socket=unix:///var/run/cri-dockerd.sock (calico)`

<br>

**For root user,**

`[anup@rhel-92-04 ~]$ export KUBECONFIG=/etc/kubernetes/admin.conf`

<br>

**For nonroot user,**


`[anup@rhel-92-04 ~]$ mkdir -p $HOME/.kube`

`[anup@rhel-92-04 ~]$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`

`[anup@rhel-92-04 ~]$ sudo chown $(id -u):$(id -g) $HOME/.kube/config`

<br>

**To join master,**


    kubeadm join 192.168.56.4:6443 --token 70gkub.z2blgf5fem7kxy8b \
            --discovery-token-ca-cert-hash sha256:fb0850c6gdab6j008cke574348b5bb5ddc9ba0a10746c8e9f7367d7723f243881a56

    anup@ubuntu-22042-16:~$ sudo kubeadm join 192.168.56.4:6443 --token b1kg09.u0hrymd5s46lgc4k --discovery-token-ca-cert-hash sha256:0babfb00e5caa29d56c5cc95adbef00f88ccd7726d2ea5053c2b7ac35ee5013a --cri-socket=unix:///var/run/cri-dockerd.sock


<br>

`[anup@rhel-92-04 ~]$ kubelet --version`
 
`[anup@rhel-92-04 ~]$ kubeadm version`

`[anup@rhel-92-04 ~]$ kubectl version`

<br>

`[anup@rhel-92-04 ~]$ kubectl version --client`

`[anup@rhel-92-04 ~]$ kubectl cluster-info`

`[anup@rhel-92-04 ~]$ kubectl version -o json`

`[anup@rhel-92-04 ~]$ strace -eopenat kubectl version`

<br>

<br>

### Installing CNI for POD networking, Installing a Pod network add-on ,

**Note: The parameter pod-network-cidr changes as per the network option.**

**Example:** The suggested CIDR for flannel and canal networks is 10.244.0.0/16 and for calico network it could be 192.168.0.0/16 , The default range that Weave Net would like to use is 10.32.0.0/12

It is not necessary to provide the parameter –pod-network-cidr for other network options like Contiv, Romana and Weavenet. However, it is must for the networks Romana and Weavenet to set /proc/sys/net/bridge/bridge-nf-call-iptables to 1 by running "sysctl net.bridge.bridge-nf-call-iptables=1" to pass bridged IPv4 traffic to iptables’ chains.


https://github.com/containernetworking/cni

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#pod-network

https://kubernetes.io/docs/concepts/cluster-administration/networking/

https://github.com/kubernetes/design-proposals-archive/blob/main/network/networking.md

https://kubernetes.io/docs/concepts/cluster-administration/addons/#networking-and-network-policy

https://kubernetes.io/docs/tasks/administer-cluster/network-policy-provider/

<br>

### Weave works,

https://www.weave.works/docs/net/latest/kubernetes/kube-addon/

**Integrating Kubernetes via the Addon,**

`[anup@rhel-92-04 ~]$ kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml`

<br>
