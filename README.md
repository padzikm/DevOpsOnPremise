# Cloud-like ready to use on-premise DevOps tools

## Kubernetes
- multinode cluster setup with separate master and nodes
- network configured with calico
- service type loadbalancer available with metallb
- dynamic persistent volumes provisioning for storageClass "nfs" (default class) available with nfs-client
- docker available on separate machine
- jenkins-x available on separate machine

## CI/CD
- jenkins master with 2 connected slaves able to build docker images
- nexus package repository
- docker and docker repository
- chartmuseum helm repository

### Prerequisites
1) on windows disable hyper-v
2) install virtualbox
3) install vagrant
4) install vagrant plugins:
  - vagrant plugin install vagrant-cachier
  - vagrant plugin install vagrant-vbguest

### Kubernetes cluster
1) adjust number of nodes in cluster in vagrantfile (line (1..4).each do |i| config.vm.define "k8s-node-#{i}" do |d|) - default is 4 nodes
2) if not running ci/cd adjust disksize for each node in vagrantfile (line .disksize.size) - default is 30gb per node and 100gb for nfs storage on dev machine
3) vagrant up
4) wait for all machines to boot up - check their status via cmd - vagrant status
5) vagrant ssh dev
6) cd /vagrant/ansible
7) ansible-playbook -i inventory/dev dev.yml -v
8) ansible-playbook -i inventory/k8s k8smaster.yml -v
9) ansible-playbook -i inventory/k8s k8snode.yml -v
10) ansible-playbook -i inventory/k8s k8sprovision.yml -v
11) k8s cluster is available via kubectl on dev vm
12) docker is available on dev vm
13) jenkins-x is available on dev vm
14) nfs-server is available on dev vm on path /nfs-k8s
15) dev vm share /vagrant path with host machine - you can copy kubectl config there and setup kubectl on host machine

### CI/CD machine
1) if not running kubernetes adjust disksize for machine in vagrantfile (line d.disksize.size = "100GB") - default is 100gb
2) vagrant up dev
3) vagrant ssh dev
4) cd /vagrant/ansible
5) ansible-playbook -i inventory/dev cicd.yml -v
6) in case of error (jenkins can sometimes not install plugins) retry previous command
7) jenkins is available from host machine on 192.168.56.89:8080 with user "admin" and password "admin"
8) nexus is available from host machine on 192.168.56.89:8081 with user "admin" and password "admin"
9) docker registry is available on dev vm as 192.168.56.89:5000
