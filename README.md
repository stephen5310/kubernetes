Kubernetes
==========

This role deploys a kubernetes cluster.

Requirements
------------

No requirements yet.

Role Variables
--------------

| Name | Default Value | Description |
| ---  | ---           | ---         |
| kubernetes_remote_user | vagrant | User used to connect to virtual machines.<br>Must have sudo without password rights
| kubernetes_version | '1.29.3' | Kubernetes version to install
| kubernetes_node_hostname | node inventory hostname | Hostname of the node
| kubernetes_apiserver_advertise_address | ssh IP address of the first controlplane node | Kubernetes api server listen IP address
| kubernetes_apiserver_bind_port | 6443 | Kubernetes api server listen port
| kubernetes_cri_socket | "unix:///var/run/containerd/containerd.sock" | CRI socket to connect to
| kubernetes_image_repository | "registry.k8s.io" | Registry used to download images of kubernetes control plane components 
| kubernetes_node_name | System hostname of the node | Name of the cluster node as defined in etcd
| kubernetes_pod_network_cidr | "10.10.0.0/16" | cidr used by pods
| kubernetes_service_cidr | "10.96.0.0/12" | cidr used by services
| kubernetes_service_dns_domain | "cluster.local" | DNS domain used by services
| kubernetes_calico_crds_definition | "https://raw.githubusercontent.com/projectcalico/calico/v3.27.2/manifests/crds.yaml" | Definition file for calico custom resource definitions
| kubernetes_calico_calicoctl_download_url | "https://github.com/projectcalico/calico/releases/latest/download/calicoctl-linux-amd64" | URL to download calicoctl
| kubernetes_calico_ippools_definitions | pool1.yaml and pool2.yaml files in role files | Calico IP pools definitions
| kubernetes_calico_certs_dir | "/etc/kubernetes/pki" | Directory to store calico certificates
| kubernetes_cni_conf_dir | "/etc/cni/net.d" | Directory for cni configurations
| kubernetes_calico_deactivate_node_to_node_mesh | true if route_reflectors has more than one node in inventory, else false | Used to determine if calico node to node mesh should be deactivated

Dependencies
------------

No dependencies yet.

Example Playbook
----------------

### Example 1: k8s + containerd + calico node mesh activated

```yaml
---
- hosts: all
  remote_user: johndoe
  become: yes
  
  vars:
    kubernetes_remote_user: johndoe

  roles:
    - role: kubernetes
...
```

### Inventory for Example 1

```ini
[controlplane]
controlplane-node1

[nodes]
worker-node1
worker-node2
worker-node3
```

### Example 2: k8s + containerd + calico node mesh deactivated

```yaml
---
- hosts: all
  remote_user: johndoe
  become: yes
  
  vars:
    kubernetes_remote_user: johndoe

  roles:
    - role: kubernetes
...
```

### Inventory for Example 2

```ini
[controlplane]
controlplane-node1

[nodes]
worker-node1
worker-node2
worker-node3

[route_reflectors]
controlplane-node1
worker-node2
```

License
-------

GPLv3

Author Information
------------------

Name: Stephen LIGUE<br>
Email: stephenligue@live.fr<br>
LinkedIn: https://www.linkedin.com/in/stephen-ligue-110847bb/<br>
Github: https://github.com/stephen5310<br>
