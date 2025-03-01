# Kubernetes master installation 

prerequisties:  

1. required instance with 2 CPU 4 GB Ram you can create VM & on AWS t2 medium instances minimum required.

2. Any Linux OS Ubuntu, Amazon Linux or RedHat 

3. minimum 3 node setup 1 Master 2 Worker 

4. All three machine interconnect with network

5. Kubernetes is not support swap so disable swap. 
    if swap available so please below commands 
     1. swapoff -a 
     2. now check swap is off by using command  "free -m" swap will be show  0 0 0
     3. after this comment your fstab entry "/etc/fstab/ comment entry (swap)

6. disable selinux if enable so run this command "getenforce" if result is enforced so open vi /etc/selinux/config and change entry targeted=permissive and save this file

7. Install Docker
         sudo apt install docker.io
      1  systemctl start docker
      2  systemctl enable docker 

8.  setup containerd
        if directory not exist create containerd directory please follow blow step.  
    6  mkdir /etc/containerd
    7  containerd config default > /etc/containerd/config.toml
    9  vi /etc/containerd/config.toml (in this file change SystemCgroup = true)
   10  systemctl restart containerd
   11  systemctl status containerd

9. setup IP Forwarding in Linux 
        13  vi /etc/sysctl.conf (insert in last "net.ipv4.ip_forward = 1") 
        14  sysctl -p (then show net.ipv4.ip_forward = 1)

10. check overlay and br_netfilter is available
         lsmod | grep overlay
         lsmod | grep br_netfilter
         if not install so install command (modprobe  overlay) & (modprobe netfilter)
11.  install kubeadm setup kubernetes repo
       open this link and follow given step https://v1-31.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
        23  curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
   24  echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
   25  apt get update
   26  apt-get update
   27  apt install kubeadm kubectl kubelet

12. setup Kubernetes master node command:
       kubeadm init --pod-network-cidr=10.244.0.0/16 
      After the installation Kubernetes give you below output we have to copy and use this all steps 

      To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.31.0.136:6443 --token ji1pjq.hyflc3gcoo6pb03x \
        --discovery-token-ca-cert-hash sha256:e30391f8b7554daac4be6e9bfe9823fbbb2a8e4f94eb48c2719527bd2ec8a3da   




13.  for every worker machine follow from step 1 to 11 after setup all things in worker copy kubeadm join output to join cluster that commad is available in your 
        output of master which we are copy in step 12 

14.  using kubectl get node command on master  both node are not ready status for changing ready status we need to install Kubernetes network pluggin 
       Apply this step in master
           kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml
           kubectl get pod -A
           kubectl get node