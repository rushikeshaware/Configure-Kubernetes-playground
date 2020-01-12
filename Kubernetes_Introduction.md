# Configure-Kubernetes-playground
My First Repository on GitHub.

-------------------------------------------------------------------------------------------
*Introduction* : **Kubernetes** is a cluster and orchestration engine for docker containers. 
In other words Kubernetes is  an open source software or tool which is used to orchestrate
and manage docker containers in cluster environment.Kubernetes is also known as k8s and 
it was developed by Google and donated to “Cloud Native Computing foundation”
            In Kubernetes setup we have one master node and multiple nodes. Cluster nodes 
is known as worker node or Minion. From the master node we manage the cluster and its 
nodes using ‘kubeadm‘ and ‘kubectl‘ command.

# Kubernetes can be installed and deployed using following methods:
1) Minikube ( It is a single node kubernetes cluster)
2) Kops ( Multi node kubernetes setup into AWS )
3) Kubeadm ( Multi Node Cluster in our own premises)

# On the Master Node following components will be installed
**API Server**           –  It provides kubernetes API using Jason / Yaml over http, 
                        states of API objects are stored in etcd
**Scheduler**            –  It is a program on master node which performs the scheduling 
                        tasks like launching containers in worker nodes based on 
                        resource availability
**Controller Manager**   –  Main Job of Controller manager is to monitor replication 
                        controllers and create pods to maintain desired state.
**etcd**                 – It is a Key value pair data base. It stores configuration 
                       data of cluster and cluster state.
**Kubectl utility**      – It is a command line utility which connects to API Server 
                       on port 6443. It is used by administrators to create pods, 
                       services etc.

# On Worker Nodes following components will be installed
**Kubelet**             – It is an agent which runs on every worker node, it connects 
                      to docker  and takes care of creating, starting, deleting containers.
**Kube-Proxy**          – It routes the traffic to appropriate containers based on ip address 
                      and port number of the incoming request. In other words we can say it 
                      is used for port translation.
**Pod**                 – Pod can be defined as a multi-tier or group of containers that are 
                      deployed on a single worker node or docker host.
                     
