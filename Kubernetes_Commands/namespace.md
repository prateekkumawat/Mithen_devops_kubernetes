#############################################
#   Namespaces in Kubernetes and Usecases   #
#############################################

Namespace in kubentenes used for achive segrigation and isolation control. Namespace help to manage your resource 
indivisual in kubentes. if we have 100 deployments or pod its difficult to manage in a same namespace so for resouces isolation we will use namespace scpoe. 

But few resouces in kubernetes like storageclass. persistent volume, and Cluster roles etc. are cluster global socpe.

Initial namespaces
Kubernetes starts with four initial namespaces: 
1. kube-system: all the master and kubenerets managment pod like api-service, etcd ,schduler etc. all runnuing in kube-system pod 
2. default: Kubernetes includes this namespace so that you can start using your new cluster without first creating a namespace. if you are not create resource by define any namespace that service resource default create in this ns. 
3.kube-public: This namespace is readable by all clients (including those not authenticated). This namespace is mostly reserved for cluster usage, in case that some resources should be visible and readable publicly throughout the whole cluster.  
4. kube-node-lease: Node leases allow the kubelet to send heartbeats so that the control plane can detect node failure.

create namespace using 2 method 
1. Cli 
kubectl create ns project1
kubectl delete ns project1

2. yaml 
apiVersion: v1   # ----> api path where resouce relate to kubenretes 
kind: Namespace  # ----> service resourcename 
metadata:        # ----> define resource details like name and labels 
  name: project1 