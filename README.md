# ansible-kubernetes-production-cluster-ubuntu

A pair of ansible scripts to setup a kubernetes cluster. Tested on Ubuntu 20 TLS.

This is the minimal setup for a complete highly available "production" cluster. 

The playbooks can be copy/pasted into your own ansible repo and built upon.

Minimum requirements
===

* 3 master nodes (2 CPU*, 2 GB memory)
* 1 worker nodes (2 CPU*, 1 GB memory)
* ansible (``apt-get install ansible``)

[*] Kubernetes CLI will refuse to run on a machine with less than 2 CPU.

Adjust the ansible inventory ``inventory/static.txt`` with your hosts. The  default expects ``kubeN.home``/``workerN.home`` with account ``user:user``. 

Setup
===

``ansible-playbook setup.yml``: Prepare the hosts, download docker and kubernetes command line tools

``ansible-playbook master.yml``: Setup the kubernetes master nodes.

``ansible-playbook nodes.yml``: Setup the kubernetes worker nodes.

It's possible to add master or worker nodes. Create the node, add to the inventory, rerun the playbook.

Uninstall
===

To remove a node: `kubeadm reset` + `rm -rf /etc/cni/net.d` + `kubectl delete node <host>`

Deploy a pod
==

    export KUBECONFIG=/etc/kubernetes/admin.conf
    kubectl get nodes
    
    # demo web application
    kubectl apply -f https://raw.githubusercontent.com/paulbouwer/hello-kubernetes/master/yaml/hello-kubernetes.yaml

    kubectl get pods
    kubectl get endpoints
    kubectl get service
    
    curl <serviceIP>:80
