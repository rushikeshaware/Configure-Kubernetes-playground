# Install Kubernetes Cluster using kubeadm
Follow this documentation to set up a Kubernetes cluster on __CentOS 7__/RPM Base O.S./Virtual machines.

This documentation guides you in setting up a cluster with one master and one node.

**Step: 1**

|Role|Hostname|IP|OS|RAM|CPU|
|----|----|----|----|----|----|
|Master|master.example.com|192.168.146.131|CentOS 7|2G|2|
|node|node.example.com|192.168.146.132|CentOS 7|1G|1|

**Step: 2**
## On both master and node
Perform all the commands as root user unless otherwise specified
### Pre-requisites
##### Update /etc/hosts
So that we can talk to each of the nodes in the cluster
```
cat >>/etc/hosts<<EOF
192.168.146.131 master.example.com master
192.168.146.132 node.example.com node
EOF
```
**Step: 3**
##### Install, enable and start docker service
Use the Docker repository to install docker.
> If you use docker from CentOS OS repository, the docker version might be old to work with Kubernetes v1.13.0 and above
```
yum install -y -q yum-utils device-mapper-persistent-data lvm2 > /dev/null 2>&1
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo > /dev/null 2>&1
yum install -y -q docker-ce >/dev/null 2>&1

systemctl enable docker
systemctl start docker
```
**Step: 4**
##### Disable SELinux
```
setenforce 0
sed -i --follow-symlinks 's/^SELINUX=enforcing/SELINUX=disabled/' /etc/sysconfig/selinux
```
**Step: 5**
##### Disable Firewall
```
systemctl disable firewalld
systemctl stop firewalld
```
**Step: 6**
##### Disable swap
```
sed -i '/swap/d' /etc/fstab
swapoff -a
```
**Step: 7**
##### Update sysctl settings for Kubernetes networking
```
cat >>/etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
```
**Step: 8**
### Kubernetes Setup
##### Add yum repository
```
cat >>/etc/yum.repos.d/kubernetes.repo<<EOF
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
        https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
```
##### Install Kubernetes
```
yum install -y kubeadm kubelet kubectl
```
##### Enable and Start kubelet service
```
systemctl enable kubelet
systemctl start kubelet
```
**Step: 9**
## On master
##### Initialize Kubernetes Cluster
```
kubeadm init --apiserver-advertise-address=192.168.146.133 --pod-network-cidr=172.20.0.0/16
```
##### Copy kube config
To be able to use kubectl command to connect and interact with the cluster, the user needs kube config file.

In my case, the user account is hrushi
```
mkdir /home/hrushi/.kube
cp /etc/kubernetes/admin.conf /home/hrushi/.kube/config
chown -R hrushi:hrushi /home/hrushi/.kube
```
**Step: 10**
##### Deploy Calico network
This has to be done as the user in the above step.
```
kubectl create -f https://docs.projectcalico.org/v3.11/manifests/calico.yaml
```
**Step: 11**
##### Cluster join command
```
kubeadm token create --print-join-command
```
## On node
##### Join the cluster
Use the output from __kubeadm token create__ command in previous step from the master server and run here.

## Verifying the cluster
##### Get Nodes status
```
kubectl get nodes
```
##### Get component status
```
kubectl get cs
```

Have Fun!!
