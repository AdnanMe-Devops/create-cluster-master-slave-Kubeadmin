# create-cluster-master-slave-Kubeadmin
Create and Manage a Kubernetes Cluster from Scratch Kubeadmin
Create and Manage a Kubernetes Cluster from Scratch

Description
will create a Kubernetes cluster that adheres to best practices from scratch using the kubeadm cluster bootstrapping utility and will then also perform several cluster management tasks including performing a backup the cluster, restoring the cluster from a backup, and upgrading Kubernetes


 Objectives

Install Kubernetes master and worker nodes including TLS bootstrapping
Implement backups and restore methodologies
Perform Kubernetes cluster upgrades
Test Kubernetes clusters
Evaluate different Kubernetes cluster configurations
 
Logging in to the Amazon Web Services Console
Connecting to an EC2 Instance Using Amazon EC2 Instance Connect
Installing kubeadm and Its Dependencies
Initializing the Kubernetes Master Node
Joining a Worker Node to the Kubernetes Cluster
Backing Up and Restoring Kubernetes Clusters
Upgrading Kubernetes Clusters with kubeadm



kubeadm is a tool that allows you to easily create Kubernetes clusters that adhere to best practices. It can also perform a variety of cluster lifecycle functions, such as upgrading and downgrading the version of Kubernetes on nodes in the cluster.  will use kubeadm to create a Kubernetes cluster from scratch . Creating clusters with kubeadm is the recommended way for learning Kubernetes, creating small clusters, and as a piece of a more complex systems for more enterprise-ready clusters.

 three EC2 instances running the Ubuntu 16.04 distribution of Linux and will configure the instance named instance-a as a Kubernetes master and the other instances as worker nodes in the cluster. In this  Step,  will install kubeadm and its dependencies, including Docker, on instance-a. The remaining nodes already been have kubeadm installed to save you time.

Initializing the Kubernetes Master Node
command 

 will use kubeadm to initialize the master node in this Step. The initialization process will create a certificate authority for secure cluster communication and authentication, and start all the node components (kubelet), master components (API server, controller manager, scheduler, etcd), and common add-ons (kube-proxy, DNS).  will see how easy the initialization process is with kubeadm. 

The initialization uses sensible default values that adhere to best practices. However, many command options are available to configure the process, including   to provide your own certificate authority or if you want to use an external etcd key-value store. One option that  will use is required by the pod network plugin that  will install after the master is initialized. kubeadm does not install a default network plugin so will use Weave as the pod network plugin. Weave supports Kubernetes network policies. For network policies to function properly,  must use the --pod-network-cidr option to specify a range of IP addresses for the pod network when initializing the master node with kubeadm. 

There are many network plugins besides Weave. Weave is used primarily because it is used in clusters in Kubernetes certification exams and it supports network policies. Weave is used internally by AWS, Azure, and GCP for their managed Kubernetes offerings, so you can be certain it is production-ready. However, if all environments live in AWS, good idea would be may consider the Amazon VPC network plugin.

 Joining a Worker Node to the Kubernetes Cluster
The process of adding a worker node with kubeadm is even simpler than initializing a master node.  will join a worker node to the cluster using the command that kubeadm init provided.

Backing Up and Restoring Kubernetes Clusters 

the state information of a Kubernetes cluster is stored in etcd. You can back up a Kubernetes cluster and restore it to an earlier state by using the snapshot and restore functionality of etcd.  will explore this functionality in this  Step and will use a command-line client named etcdctl to interact with the Kubernetes ectd key-value store. Rather than install etcdctl on the host machine, which is a viable option,  will use pods and containers to perform the backup and restore operations.

Although not a focus of this  Step, in practice  should also back up the cluster's certificate authority key (/etc/kubernetes/pki/ca.key) and certificate (/etc/kubernetes/pki/ca.crt). You should do that after the master is first initialized with kubeadm init.

Upgrading Kubernetes Clusters with kubeadm

kubeadm supports upgrading Kubernetes clusters. In this Lab Step, you will be upgrading Kubernetes from version 1.13.4 to version 1.14.1. Although upgrading is supported, you should always take care to understand any changes between releases by reading the release notes and how they could impact your workloads. You should always backup important data before upgrading, and test upgrades before deploying them to production.

The upgrade process follows the general procedure of:

Upgrading the Kubernetes control plane with kubeadm (Kubernetes components and add-ons excluding the CNI)
Manually upgrading the CNI network plugin, if applicable ( the installed version 3.1 of Calico is already the appropriate one for Kubernetes version 1.11.1)
Upgrading the Kubernetes packages (kubelet, kubeadm, kubectl) on the master and worker nodes
Upgrading the kubelet config on worker nodes with kubeadm
