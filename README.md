---

## Setting up the Kubernetes Envioroment through Vagrant

1. Installing Virtual Box

    [Dowload Virtual Box for your respective Platfom](https://www.virtualbox.org/)

2. Installing Vargarnt

    [Dowload Vagrant for your respective Platfom]https://www.vagrantup.com/downloads.html

>  You should see something as below
    
    Jagadish:k8s_centos jvemugunta$# vagrant version
   
    Installed Version: 2.2.6
    Latest Version: 2.2.6
    You're running an up-to-date version of Vagrant!

  3. Install git for your respective operting system
  
  4. Download The Repo  
  
    git clone https://github.com/jvemugunta/k8s_centos.git
    
  5. Change into Vagrant Provisioning Directory
  
    cd k8s_centos/vagrant-provisioning
    
> 

    Vagrantfile: The primary function of the Vagrantfile is to describe the type of machine required for a project, and how     to    configure and provision these machines. In this file, you will have settings such as operating system type,   hostnames of    your VMs in the cluster, node count for a few examples. The file also specifies the bootstrap scripts.

    bootstrap.sh: This file is a basic set of initial instructions that will generally be applied to both the master             Kubernetes node and the worker nodes as well. It does things such as update the /etc/hosts file to include all the   nodes,installs docker, disable selling and firewalld. It also installed Kubernetes 

    bootstrap_kmaster.sh: This file Initialize Kubernetes, create the flannel network

    bootstrap_kworker: This script will join the worker nodes to the cluster

    Almost all interaction with Vagrant is done through the command-line interface. The interface is available using the         vagrant command, and comes installed with Vagrant automatically. The vagrant command in turn has many sub commands.  
    
6. Bringing up the Kubernetes Cluster

    > 
        vagrant up
        Bringing machine 'kmaster' up with 'virtualbox' provider...
        Bringing machine 'kworker1' up with 'virtualbox' provider...
        Bringing machine 'kworker2' up with 'virtualbox' provider...
        
    
7.  Checking the Status

    >
        vagrant status
        Current machine states:
        kmaster                   running (virtualbox)
        kworker1                  running (virtualbox)
        kworker2                  running (virtualbox)
        
8.   Logging into the Master Node

    >
        vagrant ssh kmaster
        Last login: Mon Feb 24 18:37:52 2020
        [vagrant@kmaster ~]$ 
        
    >
        kubectl get nodes
        NAME                   STATUS   ROLES    AGE     VERSION
        kmaster.example.com    Ready    master   16m     v1.17.3
        kworker1.example.com   Ready    <none>   9m46s   v1.17.3
        kworker2.example.com   Ready    <none>   3m12s   v1.17.3
        
    > 
         kubectl get all --all-namespaces
            NAMESPACE     NAME                                              READY   STATUS    RESTARTS   AGE
            kube-system   pod/calico-kube-controllers-6b9d4c8765-zmg2l      1/1     Running   0          16m
            kube-system   pod/calico-node-8s668                             1/1     Running   0          3m59s
            kube-system   pod/calico-node-ft665                             1/1     Running   0          10m
            kube-system   pod/calico-node-qhh8d                             1/1     Running   0          16m
            kube-system   pod/coredns-6955765f44-8nfnj                      1/1     Running   0          16m
            kube-system   pod/coredns-6955765f44-gtbwp                      1/1     Running   0          16m
            kube-system   pod/etcd-kmaster.example.com                      1/1     Running   0          16m
            kube-system   pod/kube-apiserver-kmaster.example.com            1/1     Running   0          16m
            kube-system   pod/kube-controller-manager-kmaster.example.com   1/1     Running   0          16m
            kube-system   pod/kube-proxy-94crk                              1/1     Running   0          10m
            kube-system   pod/kube-proxy-kj4x9                              1/1     Running   0          16m
            kube-system   pod/kube-proxy-rv2kq                              1/1     Running   0          3m59s
            kube-system   pod/kube-scheduler-kmaster.example.com            1/1     Running   0          16m

            NAMESPACE     NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
            default       service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP                  16m
            kube-system   service/kube-dns     ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   16m

            NAMESPACE     NAME                         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
            kube-system   daemonset.apps/calico-node   3         3         3       3            3           beta.kubernetes.io/os=linux   16m
            kube-system   daemonset.apps/kube-proxy    3         3         3       3            3           beta.kubernetes.io/os=linux   16m

            NAMESPACE     NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
            kube-system   deployment.apps/calico-kube-controllers   1/1     1            1           16m
            kube-system   deployment.apps/coredns                   2/2     2            2           16m

            NAMESPACE     NAME                                                 DESIRED   CURRENT   READY   AGE
            kube-system   replicaset.apps/calico-kube-controllers-6b9d4c8765   1         1         1       16m
            kube-system   replicaset.apps/coredns-6955765f44                   2         2         2       16m

       

        
