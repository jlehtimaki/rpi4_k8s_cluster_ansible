# Raspberry Pi4 HA Kubernetes Cluster
## Description
This repository will install all necessary packages needed for Kubernetes.
Installation will use ansible and kubeadm with Weave CNI networking. \
Ansible will also install HAProxy with Keepalived to enable HA clustering.
## Prerequisites
- Raspberry Pi 4 with Ubuntu 19.10 (tested on this setup)
- 1 - 3 Master nodes
- X amount of worker nodes
- Ansible 2.x

### Hosts file
Hosts file should be in format (example):
```
[masters]
kube-master-1.k8s.localhost   ansible_host=192.168.1.10    keepalive_role=MASTER
kube-master-2.k8s.localhost   ansible_host=192.168.1.11    keepalive_role=SLAVE
kube-master-3.k8s.localhost   ansible_host=192.168.1.12    keepalive_role=SLAVE

[nodes]
kube-node-1.k8s.localhost   ansible_host=192.168.1.20
kube-node-2.k8s.localhost   ansible_host=192.168.1.21
kube-node-3.k8s.localhost   ansible_host=192.168.1.22
```

ansible_host parameter is not required if you have nodes in DNS.
\
Keepalive_role will determine which host acts as primary master for haproxy and keepalived.

### Variables
Variables can be defined in group_vars/all.yml\
These are current default but they can be changed if needed
```
kubeadm_version: 1.16.4-00
kubernetes_version: stable-1.16
kubernetes_endpoint: 192.168.88.111
haproxy_port: 6666
pod_cidr: 10.244.0.0/16
```

## Usage
First run playbook for masters then to nodes.
### Master
`ansible-playbook k8s-master.yml`
### Nodes
`ansible-playbook k8s-nodes.yml`
